In order to set up tests and run code coverage, you need to have certain software on your local Mac.

1. Composer—to download test libraries and PHPUnit
2. Xdebug or PCOV — to generate code coverage
3. Node.js—if you want to write JS tests for your projects

While Composer and Node should have been installed on your computer by now, the Xdebug module is most probably not installed.

## Installing Xdebug

For installing Xdebug follow the [Setting up Xdebug](/development-environment-setup/xdebug) chapter.

## Installing PCOV

[PCOV](https://github.com/krakjoe/pcov) is a self contained [CodeCoverage](https://github.com/sebastianbergmann/php-code-coverage) compatible driver for PHP7. It comes enabled on PHPUnit8 by default. However, since PHPUnit8 has introduced some typehints compatible with PHP 7.2 (or newer) only, we cannot (yet) use it for running WordPress tests (unit tests will work fine).

We can install PCOV as an extension, and use [pcov-clobber](https://github.com/krakjoe/pcov-clobber) to run it on PHPUnit 7.

If you are using Codeception, you don't need `pcov-clobber`.

Be careful where you are running your tests from. If you are running them locally, `pcov` must be installed on your local system. If you are running them in VVV or Homestead, you'll need to install `pcov` in your virtual machine.

You can install PCOV using `pecl`

```bash
pecl install pcov
```

or by cloning and compiling it manually.

```bash
git clone https://github.com/krakjoe/pcov.git
cd pcov
phpize
./configure --enable-pcov
make
make test
make install
```

Make sure you note where the extension was installed, as you'll need to specify the installation path in your `php.ini`.

Add the following to your `php.ini`:

```bash
extension=/usr/local/Cellar/php/7.4.16/pecl/20180731/pcov.so # This can vary on your system!!!
pcov.enabled = 1
```

Finally, install the `pcov-clobber` in your project using Composer:

```bash
composer require pcov/clobber --dev
```

and run `vendor/bin/pcov clobber`.

Be sure to disable the Xdebug coverage mode in your `php.ini`.
