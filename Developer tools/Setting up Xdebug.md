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

You can use Xdebug to debug your CLI scripts (tests for instance), or debug your web application.

In order to debug the web application, you'll need a browser extension. In Chrome it's Xdebug helper, in FF it's Xdebug helper for Firefox.

If you are using PhpStorm, setting up Xdebug is [straightforward](https://www.jetbrains.com/help/phpstorm/configuring-xdebug.html). In VSCode you'll need to install the PHP Debug extension, configre it, and then you should be able to set breakpoints in your code.

Be sure to enable the helper and listener in your IDE.

### Debugging CLI scripts

When you want to debug the CLI scripts, such as automated tests or custom commands, you need to add a manual trigger:

```bash
XDEBUG_TRIGGER=yes CLI SCRIPT
```

This will instruct Xdebug to be triggered on the script run.

#### WSL2 issues

Windows Subsystem for Linux (WSL) works differently than MacOS system, because you need to set the correct external IP address for Xdebug to be able to 'listen' to the requests.

To quickly find the external IP of your WSL you can type

```bash
ip route show default | awk '{print }'
```

in your WSL terminal. Then you need to add this IP address as your `xdebug.client_host` in your `php.ini` or `xdebug.ini` settings.

After that make sure you restart your PHP server. You should be able to use XDebug normally.

In order to debug the CLI scripts, you'll need to add additional environment variable

```bash
XDEBUG_TRIGGER=yes PHP_IDE_CONFIG=serverName=yourprojectname.test CLI SCRIPT
```
##### PhpStorm 

For webserver script debugging (e.g. integration test debugging), you can add the URL to your server setup, and correct port mappings in the server settings, and it should work.

In the case of running script debugging that isn't tied to a specific domain (for instance you want to debug a phpcs sniff), the server name can be localhost, but it's important to correctly set up path mappings. For instance in the image below

![Server settings for PhpStorm](/img/wsl-xdebug.png)

The name of the server is `WSL`, host is localhost, but the port mappings between the local files and server files are specified (note that the local ones have `\\wsl$` prefix, while the server ones are the ones you get by typing `pwd` inside your terminal, like `/home/...`).

Then running the script as 

```bash
XDEBUG_TRIGGER=yes PHP_IDE_CONFIG=serverName=WSL CLI SCRIPT
```

Will correctly trigger a breakpoint and you can debug your script as before.

That is because from the PhpStorm's point of view, the WSL distribution is a server, so it needs to have the correct port mappings available so that it can connect the dots.

##### Visual Studio Code 

Install the PHP Debug extension published by Xdebug. The PHP Debug extension can also be found in VSCode's Extensions tab by searching for it.

![PHP Debug Extension](/img/vsc-php-debug-extension.png)

Once installed click on the Run tab and select "Add configuration...". Now, you'll need to pick the PHP environment. A new `launch.json` file will be added to the root directory by VSCode. By default it will contain 3 configurations. The one you need is the "Listen for Xdebug".

Example of the "Listen for Xdebug" configuration:

```json
{
    "name": "Listen for Xdebug",
    "type": "php",
    "request": "launch",
    "port": 9003
}
```

Start debugging by opening the debug mode tab. In the dropdown select "Listen for Xdebug" and click the green debug button next to the dropdown.

If the process started successfully You will now see several options in the window, via which you can pick what logs Xdebugger will show like:

- Notices
- Warnings
- Errors
- Exceptions
- Everything

And you will see a small floating bar where you can Pause/Stop the debugger or step trough the steps when debugging breakpoints.

While the debugger is running you can run unit tests or run your website (chrome php debug extension needed) and debugger will listen for anything you selected and show you the data in the VS Code debug view.

### Using Xdebug in Postman

There are some instances where you'd like to debug the logic for your API calls.

In this case you'll need to add a query parameter to your endpoint URL:

```bash
https://yoururl.test/wp-json/wp/v2/posts?XDEBUG_SESSION_START=PHPSTORM
```

The query parameter value can be changed according to your IDE.
