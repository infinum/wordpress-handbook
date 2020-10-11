As stated in [the official documentation](https://laravel.com/docs/5.4/valet), Valet is a Laravel development environment for Mac minimalists.

Laravel Valet configures your Mac to always run Nginx in the background when your machine starts. Then, Valet proxies all requests on the `*.test` domain using DnsMasq to point to sites installed on your local machine.

It is ideal for machines with a small amount of RAM.

## Installing Laravel Valet

The setup for Valet is really easy.

* Install or update Homebrew to the latest version using the brew update.

* Install PHP using Homebrew via `brew install php`.

* If you don't have Composer installed, install it via `brew install composer`. Be sure to restart the shell and add the  `~/.composer/vendor/bin` directory in your system's "PATH" (see instructions below).

* Install Laravel via Composer: `composer global require laravel/installer`.

* Install Valet with Composer: `composer global require laravel/valet`.

* Run the `valet install` command. This will configure and install Valet and DnsMasq, and register Valet's daemon to launch when your system starts.

## Possible issues

### Composer not in the global system `$PATH`

When you type `echo $PATH`, you should get something like this:

```bash
/Users/infinum/.composer/vendor/bin:/usr/local/sbin:/Users/infinum/wpcs/vendor/bin:/Users/infinum/.rbenv/shims:/Users/infinum/.rbenv/bin:/Users/infinum/bin:/usr/local/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/opt/X11/bin
```

If the `/Users/infinum/.composer/vendor/bin` part is missing, it means that you need to add the composer to the global system `$PATH`, so that it is available for use through your terminal.

If you are using Z shell, you need to edit your `/.zshrc` file, and add

```bash
export PATH="$HOME/.composer/vendor/bin:$PATH"
```

to the list. Then you need to restart the terminal, and the composer should be added to your `$PATH`. If you are using `bash`, then you should do the same but in the `/.bash_profile` file.

## Setting up the site

Valet has a built-in WordPress driver, so it supports running WordPress. You can serve your sites in two ways — by using the `park` or `link` option.

The `park` option can be used to add a whole folder to Valet. Every folder in there will then be mapped to a site and get its own `.test` domain. So, if you add the `~/Sites` folder to Valet and have a subfolder in there named `wp-valet`, the content of that folder will be served when you visit `http://wp-valet.test` in your browser. No setup is required. The `link` option can be used to serve a single site, without adding a whole directory.

```bash
$ mkdir ~/Sites
$ cd ~/Sites
$ valet park
```

Then, you need to add the `wp-valet` folder and use [WP CLI](http://wp-cli.org/) to install WordPress.

```bash
$ mkdir wp-valet
$ cd wp-valet
$ wp core download
```

And this is it. Once you do this, you should be able to go to `http://wp-valet.test` and see the classic WordPress installation screen.

## Possible issues

### MySQL not started and database not set up

In the odd case that installation does not work and you get the blue 404 screen, try checking whether you are connected to the MySQL database.

```bash
brew services list
```

You should see something like this:

```bash
Name       Status  User          Plist
dnsmasq    started root          /Library/LaunchDaemons/homebrew.mxcl.dnsmasq.plist
mysql      started infinum-denis /Users/infinum/Library/LaunchAgents/homebrew.mxcl.mysql.plist
nginx      started root          /Library/LaunchDaemons/homebrew.mxcl.nginx.plist
php71      started root          /Library/LaunchDaemons/homebrew.mxcl.php71.plist
postgresql started infinum-denis /Users/infinum/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
```

If MySQL is not running, start it with

```bash
brew services start mysql
```

Log into your MySQL with `mysql -u root` and create the desired database.


```bash
create database db_name;
grant all privileges on db_name.* to 'root'@'localhost' identified by "";
flush privileges;
exit;
```

This will create a `db_name` (change this according to your project) database and add a root user to it.

You should then be able to add those details to `wp-config.php` and install WordPress.
