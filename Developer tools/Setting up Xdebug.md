Xdebug is an indispensible tool when working on PHP projects. It's a very powerful debugger which can be used for code coverage when running tests as well.

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
Installing '/usr/local/Cellar/php@7.4/7.4.16/pecl/20200930/xdebug.so'
install ok: channel://pecl.php.net/xdebug-3.0.4
Extension xdebug enabled in php.ini
```

In this case, all you need to check is whether PHP has linked the module correctly.

```bash
php -v

PHP 7.4.16 (cli) (built: Mar  4 2021 20:52:51) ( NTS )
Copyright (c) The PHP Group
Zend Engine v3.4.0, Copyright (c) Zend Technologies
    with Zend OPcache v7.4.16, Copyright (c), by Zend Technologies
    with Xdebug v3.0.4, Copyright (c) 2002-2021, by Derick Rethans
```

If you get an error message, you'll need to find your `php.ini` file.

```bash
php --ini

Configuration File (php.ini) Path: /usr/local/etc/php/7.4
Loaded Configuration File:         /usr/local/etc/php/7.4/php.ini
Scan for additional .ini files in: /usr/local/etc/php/7.4/conf.d
Additional .ini files parsed:      /usr/local/etc/php/7.4/conf.d/error_log.ini,
/usr/local/etc/php/7.4/conf.d/ext-blackfire.ini,
/usr/local/etc/php/7.4/conf.d/ext-opcache.ini,
/usr/local/etc/php/7.4/conf.d/php-memory-limits.ini
```

Go to `/usr/local/etc/php/7.4/php.ini` and edit it by adding

```bash
[xdebug]
zend_extension="xdebug.so"
xdebug.mode=debug,develop
xdebug.client_port=9003
```

at the end of the `php.ini` file.

That's it, you're ready to go.

### Unsuccessful installation

When installing Xdebug, you might get the following error:

```bash
Build process completed successfully
Installing '/usr/local/Cellar/php@7.4/7.4.16/pecl/20200930/xdebug.so'

Warning: mkdir(): File exists in System.php on line 294
PHP Warning:  mkdir(): File exists in /usr/local/Cellar/php/7.4.16/share/php/pear/System.php on line 294

Warning: mkdir(): File exists in /usr/local/Cellar/php/7.4.16/share/php/pear/System.php on line 294
ERROR: failed to mkdir /usr/local/Cellar/php/7.4.16/pecl/20180731
```

Go to your PHP installation folder (`/usr/local/Cellar/php/7.4.16/`, for instance) and type

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

You can see that `pecl` is symlinked to `/usr/local/lib/php/pecl`, but `which pecl` specified `/usr/local/bin/pecl`, which is itself a symlink to `/usr/local/Cellar/php/7.4.16/bin/pecl`. Therefore, you need to remove the symlink.

```bash
unlink pecl
```

Now you can install Xdebug again, and it should work as described in the previous case (follow the steps described above to add it correctly into your `php.ini`).

## Possible side effects

A possible side effect of running Xdebug, especially if you enable profiler output in Xdebug settings in `php.ini`, is that the file can grow very large. To prevent this, just make sure you delete it every once in a while, or simply don't enable logging.

## Using Xdebug