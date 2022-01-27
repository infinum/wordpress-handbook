[Pest](https://pestphp.com/) is a testing framework that mimics the syntax of [Jest](https://jestjs.io/).

It's a wrapper around the PHPUnit test suite with a more fluent syntax.

Installing Pest is simple:

```bash
composer require pestphp/pest --dev --with-all-dependencies
```

Then create a `tests` folder and run the init command:

```bash
./vendor/bin/pest --init
```

After that you can run Pest with:

```bash
./vendor/bin/pest
```

We are using Pest for unit testing purposes on the [Eightshift libs](https://github.com/infinum/eightshift-libs/tree/develop/tests).

Writing tests is really simple. You can use either `it` or `test` function

```php
<?php

it('makes sure test works', function() {
	$this->assertTrue(true);
});

```

This is a trivial example that doesn't actually test anything other than that the test suite is working.

Currently it's only possible to use Pest as unit test suite, for integration testing you're still better off with [wp-browser](https://wpbrowser.wptestkit.dev/).