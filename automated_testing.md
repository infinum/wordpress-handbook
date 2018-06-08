# Unit/Integration testing

Unit testing is a level of software testing where individual units/components of a software are tested. The purpose of unit tests are to validate that each unit of the software performs as designed.

Working within WordPress means that you're not actually running unit tests (unless you're working on core patches), rather integration tests.

There are two major types of testing that you can perform.

1. WordPress test suite
2. Plugin unit tests

The first one is the official tests that each core WordPress installation has to pass through before a stable release will be made. You can find them in the official repository of the WordPress core [here](https://core.trac.wordpress.org/browser/trunk/tests). For more info about that read [automated testing](https://make.wordpress.org/core/handbook/testing/automated-testing/) chapter in the core handbook.

The other one is a `wp-cli` test setup for plugin unit tests.

## WordPress testing

WordPress testing is useful when making a contribution to WordPress core. First thing you must do is to install PHPUnit (version 6, because version 7 doesn't work with WordPress yet).

In your terminal run

```sh
curl https://phar.phpunit.de/phpunit.phar -L -o phpunit.phar
chmod +x phpunit.phar
mv phpunit.phar /usr/local/bin/phpunit
```

Or you can run

```sh
brew install phpunit
chmod +x phpunit.phar
mv phpunit.phar /usr/local/bin/phpunit
```

Then check out the test repository. The WordPress tests live in the core development repository, available via SVN at:

```sh
svn co https://develop.svn.wordpress.org/trunk/ wordpress-develop
cd wordpress-develop
```

Or Git:

```sh
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

```sh
phpunit
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

```sh
npm install
```

Then run

```sh
npm install -g grunt-cli
```

After `grunt-cli` has been installed run

```sh
grunt build
```

If the build fails on `node-sass` package, try rebuilding it again

```sh
npm rebuild node-sass
```

**Notice**: The build process is currently under revision (https://core.trac.wordpress.org/ticket/43055?cversion=0&cnum_hist=23).

### Applying a patch

Make sure VVV is running and the above steps were completed. Then you can ssh to the vagrant and go to `wordpress-develop/public_html/` folder and run

```sh
grunt watch &
svn up
```

Now you’re ready to patch. Find a ticket with patches. This examples uses `#11863`.

```sh
grunt patch:11863
```

This will show the available patches that you can select (if there were multiple patches). After you select it, you can log in to `http://src.wordpress-develop.dev/` (or `http://src.wordpress-develop.test/`). You can test the effects that patch had and after you're done with it you can revert it using

```sh
svn revert -R
```

or with

```sh
svn revert -R --cl revertme .
```

You can comment on the trac ticket with the findings, or create you own patch.

### Testing outside of wordpress-develop

You can set up a custom clean installation of WordPress if you don't want to use the oficial one from VVV.
In that case, just go to public_html and run

```sh
svn co https://develop.svn.wordpress.org/trunk/ .
```

This will download all the needed files for testing and patching like the above process.

## Plugin testing

Use WP-CLI to setup our plugin’s unit tests. If you don't have wp-cli installed on your system, install it. There is a documentation on starting unit tests which you can find [here](https://make.wordpress.org/cli/handbook/plugin-unit-tests/).

Assuming you have a plugin on your testing environment, in the project root folder run

```sh
wp scaffold plugin-tests my-plugin
```
This will create several new files and folders in your plugin

```
bin/
    install-wp-tests.sh
tests/
    bootstrap.php
    test-sample.php
phpunit.xml
.travis.yml
```


To initialize the testing environment locally go to your plugin directory and run the install script

```sh
bin/install-wp-tests.sh wordpress_unit_tests root '' localhost latest
```

The install script first it installs a copy of WordPress in the `/tmp directory` (by default) as well as the WordPress unit testing tools. Then it creates a database to be used while running tests. The parameters that are passed to `install-wp-tests.sh` setup the test database. Be sure that your mysql service is up and running if you're running tests outside of VVV.

After that you can run the plugin tests by writing

```sh
phpunit
```

The wp-cli only provides one sample test

```php
class SampleTest extends WP_UnitTestCase {

  /**
   * A single example test.
   */
  function test_sample() {
    // Replace this with some actual testing code.
    $this->assertTrue( true );
  }
}
```

So you'll need to write your own tests.

### Debugging inside tests

If you want to check the output of a variable inside your test just add

```php
fwrite( STDERR, print_r( $variable, true ) );
```
