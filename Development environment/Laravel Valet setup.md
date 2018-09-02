As stated on [the official documentation](https://laravel.com/docs/5.4/valet), Valet is a Laravel development environment for Mac minimalists.

Laravel Valet configures your Mac to always run Nginx in the background when your machine starts. Then, using DnsMasq, Valet proxies all requests on the `*.dev` domain to point to sites installed on your local machine.

It is ideal for machines with low amount of RAM.

## Installing Laravel Valet

The setup for Valet is really easy.

* Install or update Homebrew to the latest version using brew update.

* Install PHP 7.1 using Homebrew via `brew install homebrew/php/php71`.

* If you don't have Composer installed, install it via `brew install composer`. Be sure to restart the shell and add  `~/.composer/vendor/bin` directory in your system's "PATH" (see below for the instructions).

* Install Laravel via Composer via `composer global require laravel/installer`.

* Install Valet with Composer via `composer global require laravel/valet`.

* Run the `valet install` command. This will configure and install Valet and DnsMasq, and register Valet's daemon to launch when your system starts.

## Possible issues

### Composer not in the global system `$PATH`

When you type `echo $PATH` you should get something like this

```sh
/Users/infinum/.composer/vendor/bin:/usr/local/sbin:/Users/infinum/wpcs/vendor/bin:/Users/infinum/.rbenv/shims:/Users/infinum/.rbenv/bin:/Users/infinum/bin:/usr/local/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin
```

If you don't have `/Users/infinum/.composer/vendor/bin` part that means that you need to add the composer to the global system `$PATH`, so that it is available to use through your terminal.

If you are using Z shell, you need to edit your `/.zshrc` file, and add

```sh
export PATH="$HOME/.composer/vendor/bin:$PATH"
```

to the list. Then you need to restart the terminal and the composer should be added to your `$PATH`. If you are using `bash` then you'd do the same but in the `/.bash_profile` file.

## Setting the site up

Valet has a built in WordPress driver, so it supports running WordPress. You can serve your sites in two ways - using `park` or `link` option.

The `park` option can be used to add a whole folder to Valet. Every folder in there will then be mapped to a site and get its own `.dev` domain. So if you add `~/Sites` folder to Valet and have a subfolder in there named `wp-valet`, the content of that folder will be served when you visit `http://wp-valet.dev` in your browser. No setup required. The `link` option can be used to serve a single site, without adding a whole directory.

```sh
$ mkdir ~/Sites
$ cd ~/Sites
$ valet park
```

Then you need to add `wp-valet` folder and use [WP CLI](http://wp-cli.org/) to install WordPress.

```sh
$ mkdir wp-valet
$ cd wp-valet
$ wp core download
```

And this is it. Once done you should be able to go to `http://wp-valet.dev` and you'd see the classic WordPress installation screen.

## Possible issues

### mysql not started and database not set up

In the odd case that the installation isn't working, and you get blue 404 screen, try seeing if you are connected to the mysql database.

```sh
brew services list
```

You should see something like this:

```sh
Name       Status  User          Plist
dnsmasq    started root          /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
mysql      started infinum-denis /Users/infinum/Library/LaunchAgents/homebrew.mxcl.mysql.plist
nginx      started root          /Library/LaunchDaemons/homebrew.mxcl.nginx.plist
php71      started root          /Library/LaunchDaemons/homebrew.mxcl.php71.plist
postgresql started infinum-denis /Users/infinum/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

If mysql is not running, start it with

```sh
brew services start mysql
```

Log into your mysql with `mysql -u root` and create a desired database


```sh
create database db_name;
grant all privileges on db_name.* to 'root'@'localhost' identified by "";
flush privileges;
exit;
```

This will create a database `db_name` (change this according to your project), and add root user to it.

You should then be able to add those details to the `wp-config.php` and install WordPress.
