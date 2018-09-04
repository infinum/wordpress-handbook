WordPress testing (core) is useful when making a contribution to WordPress core. First thing you must do is to install PHPUnit (version 6, because version 7 doesn't work with WordPress yet).

In your terminal run

```bash
curl https://phar.phpunit.de/phpunit.phar -L -o phpunit.phar
chmod +x phpunit.phar
mv phpunit.phar /usr/local/bin/phpunit
```

Or you can run

```bash
brew install phpunit
chmod +x phpunit.phar
mv phpunit.phar /usr/local/bin/phpunit
```

Then check out the test repository. The WordPress tests live in the core development repository, available via SVN at:

```bash
svn co https://develop.svn.wordpress.org/trunk/ wordpress-develop
cd wordpress-develop
```

Or Git:

```bash
git clone git://develop.git.wordpress.org/ wordpress-develop
cd wordpress-develop
```

Create an empty MySQL database. Be careful not to use existing project database, as test suite will delete all data from the designated database.

Set up a config file. Copy `wp-tests-config-sample.php` to `wp-tests-config.php`, and enter the credentials of the newly created database.

### Running the test suite

To run unit tests, you need to specify `phpunit.xml.dist` file. An example might look like

```xml
<?xml version="1.0"?>
<phpunit
  bootstrap="tests/bootstrap.php"
  backupGlobals="false"
  colors="true"
  convertErrorsToExceptions="true"
  convertNoticesToExceptions="true"
  convertWarningsToExceptions="true"
  >
  <testsuites>
    <testsuite>
      <directory prefix="test-" suffix=".php">./tests/</directory>
    </testsuite>
  </testsuites>
  <filter>
    <whitelist>
      <directory>./</directory>
      <exclude>
        <directory>./vendor</directory>
        <directory>./tests</directory>
      </exclude>
    </whitelist>
  </filter>
  <logging>
    <log type="coverage-clover" target="tests/_reports/logs/clover.xml"/>
    <log type="coverage-html" target="tests/_reports/coverage" charset="UTF-8" yui="true" highlight="true" lowUpperBound="35" highLowerBound="70" />
    <log type="testdox-text" target="tests/_reports/testdox/executed.txt"/>
    <log type="testdox-html" target="tests/_reports/testdox/testdox.html" />
  </logging>
</phpunit>
```

Here we've specified that code coverage should be created. For that to be made, Xdebug has to be installed on your system. Be mindfull that Xdebug may significantly reduce the speed of your tests.

In the root directory – next to `wp-tests-config.php`, the `tests/` folder, and the `phpunit.xml.dist` file, run:

```bash
vendor/bin/phpunit
```

Unit test symbol meaning

* . – Each dot signifies one “test” that passed.
* S means a test was skipped. This usually means that the test is only valid in certain configurations, such as when a test requires Multisite.
* F means a test failed. More output will be shown for what exactly failed and where.
* E means a test failed due to a PHP error, warning, or notice.
* I means a test was marked as incomplete.

### Testing inside VVV

VVV comes with a test ready environment `wordpress-develop/` where you can test various patches. It already has the test suite set up so you don't need to do any manual steps.

Once in the `wordpress-develop/public_html/` folder run

```bash
npm install
```

Then run

```bash
npm install -g grunt-cli
```

After `grunt-cli` has been installed run

```bash
grunt build
```

If the build fails on `node-sass` package, try rebuilding it again

```bash
npm rebuild node-sass
```

**Notice**: The build process is currently under revision (https://core.trac.wordpress.org/ticket/43055?cversion=0&cnum_hist=23).

### Applying a patch

Make sure VVV is running and the above steps were completed. Then you can ssh to the vagrant and go to `wordpress-develop/public_html/` folder and run

```bash
grunt watch &
svn up
```

Now you’re ready to patch. Find a ticket with patches. This examples uses `#11863`.

```bash
grunt patch:11863
```

This will show the available patches that you can select (if there were multiple patches). After you select it, you can log in to `http://src.wordpress-develop.dev/` (or `http://src.wordpress-develop.test/`). You can test the effects that patch had and after you're done with it you can revert it using

```bash
svn revert -R
```

or with

```bash
svn revert -R --cl revertme .
```

You can comment on the trac ticket with the findings, or create you own patch.

### Testing outside of wordpress-develop

You can set up a custom clean installation of WordPress if you don't want to use the oficial one from VVV.
In that case, just go to public_html and run

```bash
svn co https://develop.svn.wordpress.org/trunk/ .
```

This will download all the needed files for testing and patching like the above process.
