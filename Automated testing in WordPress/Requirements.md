In order to set up tests, and to run code coverage you'll need to have certain software on your local mac.

1. Composer - to download the test libraries and PHPUnit
2. Xdebug - to generate code coverage
3. Node.js - if you want to write JS tests for your projects

While Composer and Node should be installed on your computer by now, Xdebug module is most probably not installed.

## Installing Xdebug

Assuming you have installed the latest stable PHP version using [Homebrew](https://brew.sh/), you should use `pecl` to install the Xdebug. [Pecl](https://pecl.php.net/) is a repository for PHP extensions. First make sure you have it installed (even though it comes with a homebrew php version).

```bash
which pecl

/usr/local/bin/pecl
```

Try to install xdebug

```bash
pecl install xdebug
```

There are two ways that can happen: Your installation passes and you get a message like this

### Successful installation

```bash
...

Build process completed successfully
Installing '/usr/local/Cellar/php/7.3.7/pecl/20180731/xdebug.so'
install ok: channel://pecl.php.net/xdebug-2.7.2
Extension xdebug enabled in php.ini
```

In this case all you need to check if the php has linked the module correctly

```bash
php -v

PHP 7.3.7 (cli) (built: Aug  5 2019 02:51:04) ( NTS )
Copyright (c) 1997-2018 The PHP Group
Zend Engine v3.3.7, Copyright (c) 1998-2018 Zend Technologies
    with Xdebug v2.7.2, Copyright (c) 2002-2019, by Derick Rethans
    with Zend OPcache v7.3.7, Copyright (c) 1999-2018, by Zend Technologies
```

If you get an error message, you'll need to find your `php.ini` file

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

At the end of the `php.ini` file.

That's it, you're ready to go.

### Unsuccessful installation

When installing xdebug, you might get the following error

```bash
Build process completed successfully
Installing '/usr/local/Cellar/php/7.3.7/pecl/20180731/xdebug.so'

Warning: mkdir(): File exists in System.php on line 294
PHP Warning:  mkdir(): File exists in /usr/local/Cellar/php/7.3.7/share/php/pear/System.php on line 294

Warning: mkdir(): File exists in /usr/local/Cellar/php/7.3.7/share/php/pear/System.php on line 294
ERROR: failed to mkdir /usr/local/Cellar/php/7.3.7/pecl/20180731
```

Go to your php installation folder (`/usr/local/Cellar/php/7.3.7/` for instance) and type

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

You can see that `pecl` is symlinked to `/usr/local/lib/php/pecl`, but `which pecl` specified `/usr/local/bin/pecl`. Which is itself a symlink to `/usr/local/Cellar/php/7.3.7/bin/pecl`. So you need to remove the symlink

```bash
unlink pecl
```

Now you can install Xdebug again, and it should work like in the previous case (follow the steps described there to add it correctly in your `php.ini`).

## Possible sideaffects

Possible sideaffects of running Xdebug, especially if you enable profiler output in the xdebug settings in `php.ini` is that that file can grow very large, so just make sure that you delete it every once in a while, or just don't enable logging.



