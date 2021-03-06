In order to set up tests and run code coverage, you need to have certain software on your local Mac.

1. Composer—to download test libraries and PHPUnit
2. Xdebug or PCOV — to generate code coverage
3. Node.js—if you want to write JS tests for your projects

While Composer and Node should have been installed on your computer by now, the Xdebug module is most probably not installed.

## Installing Xdebug

Assuming you have installed the latest stable PHP version using [Homebrew](https://brew.sh/), you should use `pecl` to install the Xdebug. [Pecl](https://pecl.php.net/) is a repository for PHP extensions. First, make sure you have it installed (even though it comes with the Homebrew PHP version).

```bash
which pecl

/usr/local/bin/pecl
```

Try to install Xdebug:

```bash
pecl install xdebug
```

That can be done in two ways: your installation passes, and you get a message like this:

### Successful installation

```bash
...

Build process completed successfully
Installing '/usr/local/Cellar/php/7.3.7/pecl/20180731/xdebug.so'
install ok: channel://pecl.php.net/xdebug-2.7.2
Extension xdebug enabled in php.ini
```

In this case, all you need to check is whether PHP has linked the module correctly.

```bash
php -v

PHP 7.3.7 (cli) (built: Aug  5 2019 02:51:04) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.7, Copyright (c) 1998-2018 Zend Technologies
    with Xdebug v2.7.2, Copyright (c) 2002-2019, by Derick Rethans
    with Zend OPcache v7.3.7, Copyright (c) 1999-2018, by Zend Technologies
```

If you get an error message, you'll need to find your `php.ini` file.

```bash
php -i | grep .ini

Configuration File (php.ini) Path => /usr/local/etc/php/7.3
Loaded Configuration File => /usr/local/etc/php/7.3/php.ini
Scan this dir for additional .ini files => /usr/local/etc/php/7.3/conf.d
Additional .ini files parsed => /usr/local/etc/php/7.3/conf.d/ext-opcache.ini
user_ini.cache_ttl => 300 => 300
user_ini.filename => .user.ini => .user.ini
Supported handlers => ndbm cdb cdb_make inifile flatfile
com_init_db => 0
Classes => AppendIterator, ArrayIterator, ArrayObject, BadFunctionCallException, BadMethodCallException, CachingIterator, CallbackFilterIterator, DirectoryIterator, DomainException, EmptyIterator, FilesystemIterator, FilterIterator, GlobIterator, InfiniteIterator, InvalidArgumentException, IteratorIterator, LengthException, LimitIterator, LogicException, MultipleIterator, NoRewindIterator, OutOfBoundsException, OutOfRangeException, OverflowException, ParentIterator, RangeException, RecursiveArrayIterator, RecursiveCachingIterator, RecursiveCallbackFilterIterator, RecursiveDirectoryIterator, RecursiveFilterIterator, RecursiveIteratorIterator, RecursiveRegexIterator, RecursiveTreeIterator, RegexIterator, RuntimeException, SplDoublyLinkedList, SplFileInfo, SplFileObject, SplFixedArray, SplHeap, SplMinHeap, SplMaxHeap, SplObjectStorage, SplPriorityQueue, SplQueue, SplStack, SplTempFileObject, UnderflowException, UnexpectedValueException
open sourced by => Epinions.com
```

Go to `/usr/local/etc/php/7.3/php.ini` and edit it by adding

```bash
zend_extension=/usr/local/Cellar/php/7.3.7/pecl/20180731/xdebug.so
xdebug.remote_autostart=1
xdebug.remote_port=9000
xdebug.remote_enable=1
```

at the end of the `php.ini` file.

That's it, you're ready to go.

### Unsuccessful installation

When installing Xdebug, you might get the following error:

```bash
Build process completed successfully
Installing '/usr/local/Cellar/php/7.3.7/pecl/20180731/xdebug.so'

Warning: mkdir(): File exists in System.php on line 294
PHP Warning:  mkdir(): File exists in /usr/local/Cellar/php/7.3.7/share/php/pear/System.php on line 294

Warning: mkdir(): File exists in /usr/local/Cellar/php/7.3.7/share/php/pear/System.php on line 294
ERROR: failed to mkdir /usr/local/Cellar/php/7.3.7/pecl/20180731
```

Go to your PHP installation folder (`/usr/local/Cellar/php/7.3.7/`, for instance) and type

```bash
ls -all

total 168
-rw-r--r--   1 infinum-denis  staff   3.1K Aug  7 10:10 INSTALL_RECEIPT.json
-rw-r--r--   1 infinum-denis  staff   3.1K Jul  3 13:30 LICENSE
-rw-r--r--   1 infinum-denis  staff    64K Jul  3 13:30 NEWS
-rw-r--r--   1 infinum-denis  staff   1.6K Jul  3 13:30 README.md
drwxr-xr-x  12 infinum-denis  staff   384B Aug  7 10:10 bin
-rw-r--r--   1 infinum-denis  staff   628B Aug  7 10:10 homebrew.mxcl.php.plist
drwxr-xr-x   3 infinum-denis  staff    96B Jul  3 13:30 include
drwxr-xr-x   4 infinum-denis  staff   128B Jul  3 13:30 lib
lrwxr-xr-x   1 infinum-denis  staff    23B Aug  7 10:10 pecl -> /usr/local/lib/php/pecl
drwxr-xr-x   3 infinum-denis  staff    96B Jul  3 13:30 sbin
drwxr-xr-x   4 infinum-denis  staff   128B Jul  3 13:30 share
```

You can see that `pecl` is symlinked to `/usr/local/lib/php/pecl`, but `which pecl` specified `/usr/local/bin/pecl`, which is itself a symlink to `/usr/local/Cellar/php/7.3.7/bin/pecl`. Therefore, you need to remove the symlink.

```bash
unlink pecl
```

Now you can install Xdebug again, and it should work as described in the previous case (follow the steps described above to add it correctly into your `php.ini`).

## Possible side effects

A possible side effect of running Xdebug, especially if you enable profiler output in Xdebug settings in `php.ini`, is that the file can grow very large. To prevent this, just make sure you delete it every once in a while, or simply don't enable logging.

## Installing PCOV

The problem with using Xdebug to generate code coverage is that it is primarily a debugging tool. That means that it adds a huge overhead when generating code coverage, so, ultimately, the tests take a long time to execute.

Luckily for us, there is a way to shorten that time and use less memory. [PCOV](https://github.com/krakjoe/pcov) is a self contained [CodeCoverage](https://github.com/sebastianbergmann/php-code-coverage) compatible driver for PHP7. It comes enabled on PHPUnit8 by default. However, since PHPUnit8 has introduced some typehints compatible with PHP 7.2 (or newer) only, we cannot (yet) use it for running the tests.

We can install PCOV as an extension, and use [pcov-clobber](https://github.com/krakjoe/pcov-clobber) to run it on PHPUnit 7.

If you are using Codeception, you don't need `pcov-clobber`.

Be careful where you are running your tests from. If you are running them locally, `pcov` must be installed on your local system. If you are running them in VVV or Homestead, it needs to be installed in your virtual machine.

You can install PCOV using pecl

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

Make sure you note where the extension was installed as you'll need the installation path in your `php.ini`.

Then, you need to add the following to your `php.ini`:

```bash
extension=/usr/local/Cellar/php/7.3.7/pecl/20180731/pcov.so # This can vary on your system!!!
pcov.enabled = 1
```

Finally, install the `pcov-clobber` in your project using Composer:

```bash
composer require pcov/clobber --dev
```

and run `vendor/bin/pcov clobber`.

Be sure to disable the Xdebug extension in your `php.ini` (you can comment it using `;`).
