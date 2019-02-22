Setting up tests for WordPress is relatively easy. First you need to add `"phpunit/phpunit": "^6.5"` and `"brain/monkey": "^2.2"` to your composer `require-dev` part in `composer.json`.

Or you can add it from the terminal

```bash
composer require --dev brain/monkey:2.2
```

```bash
composer require --dev phpunit/phpunit:6.5
```

This will load [PHPUnit](https://phpunit.de/) and [Brain Monkey](https://brain-wp.github.io/BrainMonkey/). PHPUnit is the testing suite, and Brain Monkey is helper for testing in WordPress.

After that, in the `tests` folder add `bootstrap.php` file that looks like

```php
<?php
/**
 * PHPUnit bootstrap file
 *
 * @package My_Project
 */

/**
 * We need to include autoloader to use our plugin
 * and to use Brain Monkey for running unit test.
 */
require_once dirname( dirname( __FILE__ ) ) . '/vendor/autoload.php';
```

And dump your autoload to load all the necessary classess and files. It's also good to add the init class that you'll extend called `init-setup.php`

```php
<?php
/**
 * Class Init tests
 *
 * @package My_Project\Tests
 */

namespace My_Project\Tests;

use PHPUnit\Framework\TestCase;
use Brain\Monkey;

class InitTestCase extends TestCase {
  /**
   * Setup method necessary for Brain Monkey to function
   */
  protected function setUp() {
    parent::setUp();
    Monkey\setUp();
  }

  /**
   * Teardown method necessary for Brain Monkey to function
   */
  protected function tearDown() {
    Monkey\tearDown();
    parent::tearDown();
  }

  /**
   * This method is only set to silence warning
   */
  public function test_silence_warning() {
    $this->assertTrue( true, true );
  }
}
```

In the root of your project add `phpunit.xml.dist`

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
        <directory>./node_modules</directory>
        <directory>./vendor</directory>
        <directory>./tests</directory>
      </exclude>
    </whitelist>
  </filter>
  <logging>
    <log type="coverage-clover" target="tests/_reports/logs/clover.xml"/>
    <log type="coverage-html" target="tests/_reports/coverage" charset="UTF-8" yui="true" highlight="true" lowUpperBound="35" highLowerBound="70" />
  </logging>
</phpunit>
```

## Plugin testing

Use WP-CLI to setup our plugin’s unit tests. If you don't have WP-CLI installed on your system, install it. There is a documentation on starting unit tests which you can find [here](https://make.wordpress.org/cli/handbook/plugin-unit-tests/).

Assuming you have a plugin on your testing environment, in the project root folder run

```bash
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

```bash
bin/install-wp-tests.sh wordpress_unit_tests root '' localhost latest
```

The install script first it installs a copy of WordPress in the `/tmp directory` (by default) as well as the WordPress unit testing tools. Then it creates a database to be used while running tests. The parameters that are passed to `install-wp-tests.sh` setup the test database. Be sure that your mysql service is up and running if you're running tests outside of VVV.

After that you can run the plugin tests by writing

```bash
vendor/bin/phpunit
```

The WP-CLI only provides one sample test

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

## Possible issues

### Require error

When running `phpunit` for your plugin, outside VVV, you get error that looks like this

```bash
Warning: require_once(.../wordpress//wp-includes/class-phpmailer.php): failed to open stream: No such file or directory in .../wordpress-tests-lib/includes/mock-mailer.php on line 2

Fatal error: require_once(): Failed opening required '.../wordpress//wp-includes/class-phpmailer.php' (include_path='.:/opt/lampp/lib/php') in .../wordpress-tests-lib/includes/mock-mailer.php on line 2
```

In that case, delete the database (using sequel Pro or via terminal), delete the temporary folder where WordPress is installed (either in `/tmp/wordpress/` or somewhere in `/var/folders/..` subfolders), and run

```bash
bash bin/install-wp-tests.sh wordpress_test root '' localhost latest
```

in the terminal. Then the `phpunit` should work fine.

### Xdebugg inside VVV

For some reason, when Xdebugg is enabled in the VVV, when you run unit tests, and want to have the coverage generated, it will take an extreme amount of time to check it. In that case either disable creating code coverage

```bash
phpunit --no-coverage
```

Or run the test outside the VVV. Running tests outside of VVV with Xdebug is significantly faster (fev sec vs ~10 minutes).
