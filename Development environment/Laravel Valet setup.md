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

## Setting up the multisite

Multisite is a WordPress feature that allows you to have multiple sites managed from a single WordPress installation. Sites in WordPress multisite/network can have shared themes, plugins and users. It is an ideal solution when there is a need for multiple sites with similar design and functionality. More details on WordPress multisite feature can be found [here](https://wordpress.org/support/article/create-a-network/).

To set up a multisite on Laravel Valet you'll start with a WordPress download, create a database and initialize the `wp-config.php`. Don't install WordPress yet, or use `valet park` command, as we will need to map multiple URLs to a single directory (depending on the type of Multisite).

### Multisite with sub-domains

This type of multisite allows you to access sites with a sub-domain. The URLs will look like this:

```bash
dev.multisite-sub.test               # Main site
site-1.dev.multisite-sub.test
site-2.dev.multisite-sub.test
```

This type of multisite is more common and usually requested by the clients. It is also a bit more complicated to set up. To achieve this some changes to the server configuration are required before the multisite can be installed. You need to point many domains/subdomains to the same directory. Luckily, Valet is already prepared for that and has all the configurations for this type of multisite.

Taking the above URLs as an example we can start installing the multisite. First, create a main domain for the site by navigating to the directory and running: `valet link dev.multisite-sub`.

After that, add additional domains with: `valet link site-1.dev.multisite-sub`.

To install multisite run: `wp core multisite-install --prompt` and fill all the necessary data.

```bash
1/9 [--url=<url>]: dev.multisite-sub.test    # The main sites URL
2/9 [--base=<url-path>]: /
3/9 [--subdomains] (Y/n): Y
4/9 --title=<site-title>: Your site title
5/9 --admin_user=<username>: username        # This is the username of the Super Admin that will be created
6/9 [--admin_password=<password>]: password
7/9 --admin_email=<email>: admin@mail.com
8/9 [--skip-email] (Y/n): Y
9/9 [--skip-config] (Y/n): n                 # This will add additional constants to the wp-config.php
```

This will install the main site of your multisite. More info on this WP CLI command can be found [here](https://developer.wordpress.org/cli/commands/core/multisite-install/)

To add additional sites run: `wp site create --prompt` and fill all the necessary data. More info on this WP CLI command can be found [here](https://developer.wordpress.org/cli/commands/site/create/)

```bash
1/6 --slug=<slug>: site-1              # This needs to match the sub-domain that you registered with Valet
2/6 [--title=<title>]: Site Title
3/6 [--email=<email>]: admin@mail.com  # This will add existing or create a new Administrator user for this site
4/6 [--network_id=<network-id>]: 1     # Network to associate new site with
5/6 [--private] (Y/n): n
6/6 [--porcelain] (Y/n):
```

### Multisite with sub-directories

This type of multisite allows you to access sites with sub-directories. The URLs will look like this:

```bash
dev.multisite-sub.test               # Main site
dev.multisite-sub.test/site-1
dev.multisite-sub.test/site-2
```

Laravel Valet does not come with configuration for this setup out of the box. For this you will need to use a custom driver.
Guide for the installation of the custom drivers can be found [here](https://laravel.com/docs/8.x/valet#custom-valet-drivers).

There are multiple drivers available for Valet that enable multisite with sub-directories and they provide installation instructions:
* [WordPress Multisite Subdirectory Valet Driver from Objectiv](https://github.com/Objectivco/WordPressMultisiteSubdirectoryValetDriver)
* [Roots Laravel Valet and Bedrock Multisite](https://roots.io/guides/laravel-valet-and-bedrock-multisite/)

One thing to note is that by enabling custom driver for this type of multisite, the configuration for sub-domain multisite will be overwritten and will not work.

Other than installing custom drivers for sub-directory type of multisite, there is no difference in setting up this type of multisite from the sub-domain type of multisite. You won't need to register multiple domains or sub-domains as they don't apply for this type of multisite.

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
