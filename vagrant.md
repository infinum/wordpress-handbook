# Varying Vagrant Vagrants setup

## Installing VVV

Setting up VVV is easy. You can either follow the manuall install, as described in the [official documentation](https://varyingvagrantvagrants.org/docs/en-US/), or you can use `brew` package manager for quick and easy installation.

First install VirtualBox

`brew cask install virtualbox`

After VirtualBox installs install VMWare

`brew cask install vagrant`

Then you'll want to install few Vagrant plugins

```
vagrant plugin install vagrant-hostsupdater
vagrant plugin install vagrant-triggers
```

After you install plugins, you'll want to install the VVV in the local folder

```
cd ~
git clone -b master git://github.com/Varying-Vagrant-Vagrants/VVV.git vagrant-local
cd vagrant-local
```

This will clone the official VVV repository to your `vagrant-local` folder in the home folder.

While in your `vagrant-local` folder type

`vagrant up`

This will set up VVV for the first time. This may take some time (about 10 minutes or so on an average network). While this is happening, you can read the rest of this handbook.

What Vagrant is actually doing is downloading a packaged box with Ubuntu virtual machine and caching it for future use. After downloading, it will provision the running script that will download other packages necessary for local development.

Once it installs everything you'll be all set to work on your local WordPress. You can type

`vvv.dev`

in your browser, which will open up a screen with interesting links you can explore.

![vvv.dev screen](https://github.com/dingo-d/wordpress-handbook/blob/master/images/vagrant.png)

When you wish to close the Vagrant and save your ram just type

`vagrant halt`

Next time you start your Vagrant with `vagrant up` the cached box will start and it will boot in a minute or so. Reprovisioning your Vagrant is only necessary when adding new folders, which will be described below.

## Adding new sites

There are few things you need to do when setting up a new site. First, you need to add your site to the `vvv-custom.yml` file. It is just a modified copy of the existing `vvv-config.yml` file.

Let's say we want to create `wordpress-infinum` site. After the

```
wordpress-develop:
    repo: https://github.com/Varying-Vagrant-Vagrants/vvv-wordpress-develop.git
    hosts:
      - src.wordpress-develop.dev
      - build.wordpress-develop.dev
```

code in the file add

```
wordpress-infinum:
    hosts:
      - wordpress-infinum.dev
```

We've told Vagrant that inside `www/wordpress-infinum` there should be a site it can access. So we need to create it. Inside that folder we need to create two additional folders: `provision` and `public_html`.

`provision` folder holds the scripts that will set up the database and nginx configuration. `public_html` folder holds the WordPress installation.

First file that you'll add to the `provision` folder will be `vvv-init.sh`

```
#!/usr/bin/env bash
# Provision WordPress Stable

# Make a database, if we don't already have one
echo -e "\nCreating database 'wordpress_infinum' (if it's not already there)"
mysql -u root --password=root -e "CREATE DATABASE IF NOT EXISTS wordpress_infinum"
mysql -u root --password=root -e "GRANT ALL PRIVILEGES ON wordpress_infinum.* TO wp@localhost IDENTIFIED BY 'wp';"
echo -e "\n DB operations done.\n\n"

# Nginx Logs
mkdir -p ${VVV_PATH_TO_SITE}/log
touch ${VVV_PATH_TO_SITE}/log/error.log
touch ${VVV_PATH_TO_SITE}/log/access.log

# Install and configure the latest stable version of WordPress
if [[ ! -d "${VVV_PATH_TO_SITE}/public_html" ]]; then

  echo "Downloading WordPress Stable, see http://wordpress.org/"
  cd ${VVV_PATH_TO_SITE}
  curl -L -O "https://wordpress.org/latest.tar.gz"
  noroot tar -xvf latest.tar.gz
  mv wordpress public_html
  rm latest.tar.gz
  cd ${VVV_PATH_TO_SITE}/public_html

  echo "Configuring WordPress Stable..."
  noroot wp core config --dbname=wordpress_infinum --dbuser=wp --dbpass=wp --quiet --extra-php <<PHP
// Match any requests made via xip.io.
if ( isset( \$_SERVER['HTTP_HOST'] ) && preg_match('/^(wordpress-infinum.)\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}(.xip.io)\z/', \$_SERVER['HTTP_HOST'] ) ) {
    define( 'WP_HOME', 'http://' . \$_SERVER['HTTP_HOST'] );
    define( 'WP_SITEURL', 'http://' . \$_SERVER['HTTP_HOST'] );
}

define( 'WP_DEBUG', true );
PHP

  echo "Installing WordPress Stable..."
  noroot wp core install --url=wordpress-infinum.dev --quiet --title="Local Infinum WordPress Dev" --admin_name=admin --admin_email="admin@local.dev" --admin_password="password"

else

  echo "Updating WordPress Stable..."
  cd ${VVV_PATH_TO_SITE}/public_html
  noroot wp core update

fi
```

VVV uses Nginx as a web server, so the second file we need is the `vvv-nginx.conf`

```
server {
  listen 80;
  listen 443 ssl;
  server_name wordpress-infinum.com;
  root {vvv_path_to_site};

  error_log {vvv_path_to_site}/log/error.log;
  access_log {vvv_path_to_site}/log/access.log;

  set $upstream {upstream};

  include /etc/nginx/nginx-wp-common.conf;
}
```

Any time we add a new site to `vvv-custom.yml`, or the provisioner files, we need to reprovision the VVV. To do this you need to run

`vagrant reload --provision`

After that, either import the database via phpMyAdmin or wp-cli, or start from scratch.


