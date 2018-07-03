# Codeception

Codeception collects and shares best practices and solutions for testing PHP web applications. With a flexible set of included modules tests are easy to write, easy to use and easy to maintain. Codeception encourages developers and QA engineers to concentrate on testing and not on building test suite.

## Codeception for WordPress

The full installation instructions can be found [here](https://codeception.com/for/wordpress).

First install latest stable WPBrowser package via Composer

```bash
composer require lucatume/wp-browser --dev
```

Then, while in the root of your plugin or theme where you've installed the codeception run

```bash
vendor/bin/codecept init wpbrowser
```

This will set up a scaffold for setting up various tests in your plugin/theme.
