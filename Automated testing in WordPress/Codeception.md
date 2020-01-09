Codeception collects and shares recommended practices and solutions for testing PHP web applications. With a flexible set of included modules, tests are easy to write, easy to use, and easy to maintain. Codeception encourages developers and QA engineers to focus on testing and not on building a test suite.

## Codeception for WordPress

You can find full installation instructions [here](https://codeception.com/for/wordpress).

First, install the latest stable WPBrowser package via Composer.

```bash
composer require lucatume/wp-browser --dev
```

Then, while in the root of your plugin or theme where you've installed the codeception, run

```bash
vendor/bin/codecept init wpbrowser
```

This will set up a scaffolding for setting up various tests in your plugin/theme.
