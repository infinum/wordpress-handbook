Codeception collects and shares recommended practices and solutions for testing PHP web applications. With a flexible set of included modules, tests are easy to write, easy to use, and easy to maintain. Codeception encourages developers and QA engineers to focus on testing and not on building a test suite.

## Codeception for WordPress

You can find full installation instructions [here](https://wpbrowser.wptestkit.dev/).

### Installation

First, install the latest stable `WPBrowser` package via Composer in the root folder of your project. If your project is a plugin then it should be installed in the root folder of your plugin; if your project is a theme it should be installed in the root folder of your theme.

```bash
composer require --dev lucatume/wp-browser
```

If you want it to work with Codeception v4 you'll need to require these packages as well:

```json
"codeception/lib-innerbrowser": "^1.0",
"codeception/module-asserts": "^1.1",
"codeception/module-cli": "^1.0",
"codeception/module-db": "^1.0",
"codeception/module-filesystem": "^1.0",
"codeception/module-phpbrowser": "^1.0",
"codeception/module-rest": "^1.2",
"codeception/module-webdriver": "^1.0",
"codeception/util-universalframework": "^1.0",
```

You'll need to set up a test instance. You can use your preferred method of setting up development environments: Valet+, VVV, Docker, Local by Flywheel, etc. . It's important to have a working WordPress instance. That way you can run acceptance and integration tests on it.

Then, while in the root of your plugin or theme where you've installed the Codeception, run

```bash
vendor/bin/codecept init wpbrowser
```

This will set up scaffolding for setting up various tests in your plugin/theme. By default, you'll have: acceptance, functional, integration, and unit tests set up.

### Setup

The scaffold will add `.env.testing`, and `codeception.dist.yml` files to your project root, and a `tests` folder with subfolders for helpers and test suites.

#### .env.testing

This is the test environment variable file. This file can be committed to the repository, but will need to be changed depending on the user setup. For instance, if you want to use VVV for running acceptance tests, and local installation for running integration tests, you can set it up like this:

```
WP_ROOT_FOLDER="/your-folder/place-where-wp-is-installed/"

# For acceptance and functional tests
TEST_SITE_DB_DSN=mysql:host=localhost;dbname=wordpresstest
TEST_SITE_WP_ADMIN_PATH="/wp-admin"
TEST_SITE_DB_NAME="wordpresstest"
TEST_SITE_DB_HOST="localhost"
TEST_SITE_DB_USER="root"
TEST_SITE_DB_PASSWORD=
TEST_SITE_TABLE_PREFIX="wp_"
TEST_SITE_WP_URL="https://dev.wordpress.test/"
TEST_SITE_WP_DOMAIN="dev.wordpress.test"
TEST_SITE_ADMIN_EMAIL="admin@local.test"
TEST_SITE_ADMIN_USERNAME="admin"
TEST_SITE_ADMIN_PASSWORD="password"

# For integration tests
TEST_DB_NAME="wordpress-test"
TEST_DB_HOST="localhost"
TEST_DB_USER="root"
TEST_DB_PASSWORD=
TEST_TABLE_PREFIX="wp_"
```

You'll change these depending on what your setup looks like (documentation has the details for specific setups explained).

> Make sure you are not using the local database where you are developing as all the data will be lost once you run the tests.

#### codedeption.dist.yml

This file is the main test suite definition file. It contains information about the paths, settings, coverage, etc. For instance, it can look like this:

```yml
paths:
    tests: tests
    output: tests/_output
    data: tests/_data
    support: tests/_support
    envs: tests/_envs
actor_suffix: Tester
settings:
    colors: true
    memory_limit: 1024M
extensions:
    enabled:
        - Codeception\Extension\RunFailed
    commands:
        - Codeception\Command\GenerateWPUnit
        - Codeception\Command\GenerateWPRestApi
        - Codeception\Command\GenerateWPRestController
        - Codeception\Command\GenerateWPRestPostTypeController
        - Codeception\Command\GenerateWPAjax
        - Codeception\Command\GenerateWPCanonical
        - Codeception\Command\GenerateWPXMLRPC
params:
    - .env.testing
coverage:
    enabled: true
    include:
        - src/*
    low_limit: 30
    high_limit: 75
```

#### acceptance.suite.yml, functional.suite.yml, wpunit.suite.yml, unit.suite.yml

These files are located in the `tests` folder. They hold configuration for every specific test suite. For instance the integration test configuration `wpunit.suite.yml` can look like this:

```yml
# Codeception Test Suite Configuration
#
# Suite for unit or integration tests that require WordPress functions and classes.

actor: WpunitTester
bootstrap: _bootstrap.php
modules:
    enabled:
        - WPLoader
        - \Helper\Wpunit
    config:
        WPLoader:
            wpRootFolder: "%WP_ROOT_FOLDER%"
            dbName: "%TEST_DB_NAME%"
            dbHost: "%TEST_DB_HOST%"
            dbUser: "%TEST_DB_USER%"
            dbPassword: "%TEST_DB_PASSWORD%"
            tablePrefix: "%TEST_TABLE_PREFIX%"
            domain: "%TEST_SITE_WP_DOMAIN%"
            adminEmail: "%TEST_SITE_ADMIN_EMAIL%"
            title: "Test"
            plugins: ['your-plugin-dependency/your-plugin-dependency.php', 'your-plugin/your-plugin.php']
            activatePlugins: ['your-plugin-dependency/your-plugin-dependency.php', 'your-plugin/your-plugin.php']

```

It uses the configuration from the `.env.testing` file (the `%VAR%` variables). These shouldn't be changed here. You can define the plugin dependencies and which plugins you want to have activated on the site during the integration test run.
You can also define the bootstrap file that will be run before the test starts. In this case, the Codeception will look for `_bootstrap.php` inside the `wpunit` folder.

#### Composer tips

You can add composer scripts so that you can access certain test scripts easier. For instance, you can add

```json
"scripts": {
  "selenium:start": "selenium-server -port 4444",
  "test:clean": "@php ./vendor/bin/codecept clean",
  "test:acceptance": "@php ./vendor/bin/codecept run acceptance",
  "test:functional": "@php ./vendor/bin/codecept run functional",
  "test:integration": "@php ./vendor/bin/codecept run wpunit",
  "test:coverage": "@php ./vendor/bin/codecept run wpunit --coverage --coverage-xml --coverage-html",
  "test:generate-scenarios": "@php vendor/bin/codecept generate:scenarios"
},
```

`selenium:start` - used for starting a selenium server - used for acceptance and functional tests.
`test:clean` - used to clean up the output directory and generated code.
`test:setup` - a shorthand for setting up the `wpbrowser`. If you have used it already (and have test scaffolded) you don't need to run it.
`test:acceptance`- a shorthand way to run acceptance tests.
`test:functional`- a shorthand way to run functional tests.
`test:integration`- a shorthand way to run integration tests.
`test:coverage`- a shorthand way to run integration tests with code coverage (this run is usually slower than just test run).
`test:generate-scenarios`- a shorthand way to generate user scenarios. You need to provide a suite for which a scenario can be generated (`acceptance`, `functional` or `integration`).

You can also [autoload](https://getcomposer.org/doc/01-basic-usage.md#autoloading) your test folder using either `psr-4` or `classmap` autoloading process:

PSR-4:

```json
"autoload-dev": {
  "psr-4": {
    "Tests\\": "tests/"
  }
}
```

Classmap:

```json
"autoload-dev": {
  "classmap": {
    "tests/"
  }
}
```

### Levels of testing

For in depth explanation check the [official wp-browser documentation](https://wpbrowser.wptestkit.dev/v3/levels-of-testing/).

#### Acceptance tests

Acceptance and functional tests are very similar, with a distinction that acceptance tests are __testing the functionality of the project from the viewpoint of the business user__.
They are very similar to end to end (E2E) tests, but those are performed from the viewpoint of the QA engineer.

In the context of Agile development, acceptance tests should correspond to the acceptance criteria of a user story. For example, we could have a story with an acceptance criteria that a custom post type called `Books` exists. An acceptance test for this story would look like:

```php
// tests/acceptance/CustomPostType
<?php

namespace Tests\Acceptance\CustomPostType;

use AcceptanceTester;

class BooksCustomPostTypeCest
{
  public function _before(AcceptanceTester $I)
  {
    // I can activate the plugin successfully.
    $I->loginAsAdmin();
    $I->amOnPluginsPage();
    $I->seePluginInstalled('my-custom-plugin');
    $I->activatePlugin('my-custom-plugin');
    $I->seePluginActivated('my-custom-plugin');
  }

  public function _after(AcceptanceTester $I)
  {
    // Test cleanup.
  }

  public function seeBooksCustomPostTypePage(AcceptanceTester $I)
  {
    // I go to the Books admin page, and I should be able to see the title of the CPT.
    $I->amOnPage('/wp-admin/edit.php?post_type=book');
    $I->see('Books');

    // Here other acceptance criteria can be added (see columns, create new post and add content and see it successfully created, etc.).
  }
}
```

You can also write acceptance tests without classes, but they look neater this way (Cest format). The official [Codeception documentation](https://codeception.com/docs/GettingStarted) has a good introduction about the syntax, and some detailed explanation of the [Cest classes](https://codeception.com/docs/AdvancedUsage).

#### Functional tests

The functional tests test the __functionality from the perspective of a developer__.

Here you could test some custom validation rules, Ajax requests, and similar.

#### Integration tests

These tests will __test code modules in the context of a WordPress app__.

You would test each of your functionality (usually split across different functions) using integration tests. For instance, say we have created a custom rest route that will handle the contact form functionality - you would POST some data to it, and, depending on the data, expect some result. During this test, a lot of your code functionalities will be tested - rest setup and the existence of the correct route, route validation, your business logic that handles data sent to the route, email sending, etc.

In your `tests/wpunit/Routes` you could create a `ContactRouteTest.php` file

```php
<?php

namespace Tests\WPUnit\Routes;

use Codeception\TestCase\WPTestCase;
use MyPlugin\Routes\Endpoints\Contact;

class ContactRouteTest extends WPTestCase
{
  private $request;
  private $route_name;

  public function setUp(): void
  {
    parent::setUp();

    // Set up a REST server instance.
    global $wp_rest_server;

    $this->server = $wp_rest_server = new \WP_REST_Server;
    do_action( 'rest_api_init' );

    // Helper.
    $this->route_name = '/' . Contact::NAMESPACE_NAME . Contact::VERSION . Contact::ROUTE_NAME;

    $this->request = new \WP_REST_Request( 'POST', $this->route_name );
  }

  public function tearDown(): void
  {
    // Test cleanup.
    global $wp_rest_server;
    $wp_rest_server = null;

    parent::tearDown();
  }

  public function testRouteIsRegistered()
  {
    $routes = $this->server->get_routes();

    $this->assertArrayHasKey($this->route_name, $routes);
  }

  public function testRequestWasSuccessful()
  {
    $this->request->add_header('Content-Type', 'application/json;charset=utf-8');
    $this->request->set_body('{"name":"Test Testovski", "email":"test.email@email.com", "message":"Sample Message", "recaptcha":"some_random_string"}');

    $response = $this->server->dispatch( $this->request );
    $data = $response->get_data();

    $this->assertEquals( $data['http_response'], 204 );
    $this->assertEquals( $data['message'], 'Contact us email is sent. You will be contacted soon.' );
  }

  public function testHoneypotResponseIsTriggered()
  {
    $this->request->add_header('Content-Type', 'application/json;charset=utf-8');
    $this->request->set_body('{"website":"true", "name":"Test Testovski", "email":"test.email@email.com", "message":"Sample Message", "recaptcha":"some_random_string"}');

    $response = $this->server->dispatch( $this->request );
    $data = $response->get_data();

    $this->assertEquals( $data['code'], 403 );
    $this->assertEquals( $data['message'], 'bot_detected' );
  }

  // Other cases would go here.

}
```

In the context of WordPress, integration might also mean testing that filters used by the code have the expected effect.

#### Unit tests

Unit tests always __test single classes or functions (units) in isolation__.

Say we have a validator class that validates email. We would want to make sure that class works as expected, regardless if it's in the WordPress context or not. Unit tests are where stubbing/mocking/spying of dependencies is used to gain total control over the input and context the class is using.

### Setting up acceptance/feature testing environment

While wp-browser comes with a `WPBrowser` module which will help you run acceptance and functional tests, that module simulates user interaction with the site **without JavaScript support**. If you need to test your project with JavaScript support, you'll need to use the `WPWebDriver` module.

Detailed instructions are located here: https://wpbrowser.wptestkit.dev/v3/modules/WPWebDriver/

In order to run that module, you'll need to set it up in your `suite.yml` files like

```yaml
WPWebDriver:
    url: '%TEST_SITE_WP_URL%'
    adminUsername: '%TEST_SITE_ADMIN_USERNAME%'
    adminPassword: '%TEST_SITE_ADMIN_PASSWORD%'
    adminPath: '%TEST_SITE_WP_ADMIN_PATH%'
    browser: chrome
    host: localhost
    port: 9515
    window_size: false #disabled for Chrome driver
    capabilities:
        # Used in more recent releases of Selenium.
        "goog:chromeOptions":
            args: ["--no-sandbox", "--headless", "--disable-gpu", "--user-agent=wp-browser", "allow-insecure-localhost", "--ignore-certificate-errors"]
        # Support the old format for back-compatibility purposes.
        "chromeOptions":
            args: ["--no-sandbox", "--headless", "--disable-gpu", "--user-agent=wp-browser", "allow-insecure-localhost", "--ignore-certificate-errors"]
```

There is a catch, though. You'll need a web driver and a framework in which you can run that web driver. In this case, we'll use [Selenium](https://www.selenium.dev/). [Chrome Webdriver](https://developer.chrome.com/docs/chromedriver) should be installed, but doesn't have to be run, like Selenium.

Selenium can be downloaded using `brew`

```bash
brew install selenium-server-standalone
```

While for Chrome webdriver you'd need to go to [downloads section](https://googlechromelabs.github.io/chrome-for-testing/), and download **the same version of the driver as is your local Chrome**. This is important because otherwise, the tests won't be able to run - if you have v83 of Chrome, your webdriver has to be v83 as well.
Be careful about that when updating your local Chrome.

After downloading the correct zip file, extract it and move it to the local bin directory

```bash
mv chromedriver /usr/local/bin
```

Make sure you allow the driver in the MacOS security settings (Preference -> Security). You can test if it works by running

```bash
chromedriver --version

ChromeDriver 83.0.4103.39 (ccbf011cb2d2b19b506d844400483861342c20cd-refs/branch-heads/4103@{#416})
```

Similarly, you can do for Selenium

```bash
selenium-server --version

Selenium server version: 3.141.59, revision: e82be7d358
```

If your URL points to `https` local url, it's a good idea to add `"allow-insecure-localhost"` and `"--ignore-certificate-errors"` to chrome options to avoid the certificate issues that can happen when using local certificates.
