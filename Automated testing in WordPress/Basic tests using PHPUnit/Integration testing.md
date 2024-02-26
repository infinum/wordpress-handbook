Integration tests are used to check how components work inside WordPress.

The best way to set up an integration test is to use either [WPBrowser]((https://wpbrowser.wptestkit.dev/)) or WP-CLI.

## WP-CLI

If you don't have WP-CLI installed on your system, install it. You can find the documentation on starting unit tests [here](https://make.wordpress.org/cli/handbook/plugin-unit-tests/).

Assuming you have a plugin on your testing environment, run the following in the project root folder:

```bash
wp scaffold plugin-tests my-plugin
```
This will create several new files and folders in your plugin:

```
bin/
    install-wp-tests.sh
tests/
    bootstrap.php
    test-sample.php
phpunit.xml
.travis.yml
```

To initialize the testing environment locally, go to your plugin directory and run the install script.

```bash
bin/install-wp-tests.sh wordpress_unit_tests root '' localhost latest
```

The install script first installs a copy of WordPress in the `/tmp directory` (by default) as well as the WordPress unit testing tools. Then it creates a database to be used while running tests. The parameters that are passed to `install-wp-tests.sh` set up the test database. Be sure that your MySQL service is up and running if you're running tests outside VVV.

After that, you can run the plugin tests by writing

```bash
vendor/bin/phpunit
```

## Testing tips

### Testing AJAX callbacks

WordPress test suite comes equipped with a way to test WP AJAX callbacks. In order to utilize it, you'll need to create a test class that extends the `WP_Ajax_UnitTestCase` class.

This class provides a method that handles AJAX request

```php
$this->_handleAjax('ajax_action_name');
```

What this will do is, it will run the `wp_ajax_*` action, and invoke the callback. In your AJAX callbacks you either use `wp_die` or `wp_send_json_*` functions to return the contents of the callback. Since `die` directive will kill the PHP execution, WordPress  catches the `wp_die`s and prevents it from killing our tests.
Instead of killing the script execution, it will throw the exception. There are two different exceptions that can be thrown:

```php
WPAjaxDieStopException
```

in the case there was an output from the callback, and

```php
WPAjaxDieContinueException
```

in case where there was no output.

You can catch them by either setting that you are expecting an exception

```php
$this->setExpectedException('WPAjaxDieStopException');
$this->_handleAjax('ajax_action_name');
```

or, if you need to run some assertions afterward you'd catch the exception it manually

```php
try {
  $this->_handleAjax('ajax_action_name');
} catch (\WPAjaxDieStopException $e) {
  // We expected this, do nothing.
}

// Check that the exception was thrown.
$this->assertTrue(isset($e));

// The output should be a 1 for success.
$this->assertEquals('1', $e->getMessage());

$this->assertEquals('yes', get_option('some_option'));
```

If you are outputting the data using `wp_send_json_*` functions, you'll need to check the last response, where this output will be stored

```php
try {
  $this->_handleAjax('ajax_action_name');
} catch (\WPAjaxDieContinueException $e) {
  // We expected this, do nothing.
}

$response = json_decode($this->_last_response);

$this->assertEquals('Some message is outputted.', $response->data);
```

You can also set the roles of the user running triggering the callback before running the AJAX test. You'd do this by calling

```php
$this->_setRole('subscriber'); // Or other role you want to check.
```

before calling `_handleAjax()` method.

One last thing that is recommended is to group the AJAX testing class with the `@group ajax` docblock. This is not necessary, but could prove useful if you want to exclude those tests, or run them separately.
AJAX tests are slow because all the default AJAX callbacks in WordPress core get hooked up before each test is run.

## Useful links

* [Codeception for WordPress - wp-browser](https://wpbrowser.wptestkit.dev/)
* [Four WordPress Integration Testing Easy Pieces](https://www.theaveragedev.com/four-wordpress-integration-testing-easy-pieces/)
* [Integration Tests with WordPress & PHPUnit](https://aaemnnost.tv/2016/07/29/integration-tests-wordpress-phpunit/)
* [WP Ajax Plugin Unit Testing](https://codesymphony.co/author/jdgrimesdev/page/4/)
