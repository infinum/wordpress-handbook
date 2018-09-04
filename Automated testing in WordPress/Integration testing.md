Integration tests check how components work inside WordPress.

For integration test setup the best way to set it up is either using [Codeception]((https://codeception.com/for/wordpress)), or WP-CLI.

## WP-CLI

If you don't have WP-CLI installed on your system, install it. There is a documentation on starting unit tests which you can find [here](https://make.wordpress.org/cli/handbook/plugin-unit-tests/).

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

## Useful links

[Codeception for WordPress](https://codeception.com/for/wordpress)
[Four WordPress integration testing easy pieces](https://www.theaveragedev.com/four-wordpress-integration-testing-easy-pieces/)
[Integration Tests with WordPress & PHPUnit](https://aaemnnost.tv/2016/07/29/integration-tests-wordpress-phpunit/)
