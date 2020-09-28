## Installing VVV

Setting up VVV is easy. You can either follow the manual install instructions, as described in the [official documentation](https://varyingvagrantvagrants.org/docs/en-US/), or you can use the `brew` package manager for quick and easy installation.

First, install [VirtualBox 5.x](https://www.virtualbox.org/wiki/Downloads).

`brew cask install virtualbox`

After VirtualBox is installed, install [Vagrant 2.1+](https://www.vagrantup.com/downloads.html).

`brew cask install vagrant`

Then you'll want to install a few Vagrant plugins.

```sh
vagrant plugin install vagrant-hostsupdater
```

It is recommended to reboot your computer to avoid any networking issues.

After you install the plugins, you'll want to install VVV in the local folder.

```sh
cd ~
git clone -b master git://github.com/Varying-Vagrant-Vagrants/VVV.git ~/vagrant-local
cd vagrant-local
```

This will clone the official VVV repository to your `vagrant-local` folder in the home folder. If you want the latest updates and features, it is recommended to switch to the `develop` branch instead of the `master` branch.

Before starting VVV, go to `Vagrantfile` and uncomment the `config.vm.network :public_network` line. This will enable you to debug across devices later on.

You can either set up your custom sites by creating a copy of the `vvv-config.yml` file and renaming it to `vvv-custom.yml`, or you can just start Vagrant.

While in your `vagrant-local` folder, type

`vagrant up`

This will set VVV up for the first time. This may take some time (about 10 minutes on an average network). While VVV is setting up, you can read the rest of this handbook.

What Vagrant is actually doing is downloading a packaged box with the Ubuntu virtual machine and caching it for future use. After downloading, it will provision the running script that will download other packages necessary for local development.

Once it has installed everything, you'll be all set to work on your local WordPress. You can type

`http://vvv.test`

in your browser, which will open a screen with some interesting links you can explore.

![vvv.test screen](/img/vagrant.png)

When you want to close Vagrant and save your RAM, just type

`vagrant halt`

The next time you start your Vagrant with `vagrant up`, the cached box will start and boot in a minute or so. Re-provisioning your Vagrant is necessary only when adding new folders or changing your Vagrant configuration, which will be described below.

## Making your Vagrant public-friendly

Modern web development is *mobile first* oriented. With that in mind, it is natural that you want to be able to see what you are developing locally on your mobile phone.

To do that, you need to change your `Vagrantfile` located in the `vagrant-local` folder. Search for

`config.vm.network :public_network`

and uncomment it. You can also enable port forwarding while you're at it:

`config.vm.network "forwarded_port", guest: 80, host: 8888`

Now you need to re-provision your Vagrant.

`vagrant reload --provision`

When you do that, you'll be asked which network interface you use to connect to the Internet.

```sh
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Available bridged network interfaces:
1) en0: Wi-Fi (AirPort)
2) en1: Thunderbolt 1
3) en2: Thunderbolt 2
4) bridge0
5) p2p0
6) awdl0
==> default: When choosing an interface, it is usually the one that is
==> default: being used to connect to the internet.
    default: Which interface should the network bridge to?
```

In our case, we connect via Wi-Fi, so choose 1. If you're connecting via Ethernet, you'll need to select that as your network bridge.

### Using Webpack and BrowserSync Plugin

Here at Infinum, we use [eightshift-boilerplate](https://github.com/infinum/eightshift-boilerplate) to kick-start our development. It is a modern method that uses [Webpack](https://webpack.js.org/) to bundle your assets.

By using it, you'll be able to use [BrowserSync](https://www.npmjs.com/package/browser-sync-webpack-plugin) to test the development in your browser and on a mobile phone. Just follow the instructions in the `eightshift-boilerplate` repo, and you should be able to easily inspect your site without much hassle.

## Adding new sites

The official documentation on adding a new site can be found [here](https://varyingvagrantvagrants.org/docs/en-US/adding-a-new-site/). An easier way to provision a new site is by using [site templates](https://varyingvagrantvagrants.org/docs/en-US/site-templates/).
A site template is a Git repo that contains scripts and files necessary to set up a new VVV site automatically.

You can also create provision scripts manually. First, you need to add your site to the `vvv-custom.yml` file. It is just a modified copy of the existing `vvv-config.yml` file.

Let's say we want to create a `wordpress-infinum` site. After the

```yaml
wordpress-develop:
    repo: https://github.com/Varying-Vagrant-Vagrants/vvv-wordpress-develop.git
    hosts:
      - src.wordpress-develop.test
      - build.wordpress-develop.test
```

code in the file add

```yaml
wordpress-infinum:
    vm_dir: /srv/www/personal/wordpress-infinum
    local_dir: www/personal/wordpress-infinum
    hosts:
      - wordpress-infinum.test
```

We've told Vagrant that there should be a site it can access inside `www/wordpress-infinum`. So we need to create it. We need to create two additional folders inside that folder — `provision` and `public_html`.

The `provision` folder holds scripts that will set up the database and Nginx configuration. The `public_html` folder holds the WordPress installation.

The first file you will add to the `provision` folder will be `vvv-init.sh`

```sh
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

define( 'WP_DEBUG', true );
PHP

  echo "Installing WordPress Stable..."
  noroot wp core install --url=wordpress-infinum.test --quiet --title="Local Infinum WordPress Dev" --admin_name=admin --admin_email="admin@local.test" --admin_password="password"

else

  echo "Updating WordPress Stable..."
  cd ${VVV_PATH_TO_SITE}/public_html
  noroot wp core update

fi
```

VVV uses Nginx as a web server, so the second file we need is `vvv-nginx.conf`.

```sh
server {
  listen 80;
  listen 443 ssl;
  server_name wordpress-infinum.test;
  root {vvv_path_to_site}/public_html;

  error_log {vvv_path_to_site}/log/error.log;
  access_log {vvv_path_to_site}/log/access.log;

  set $upstream {upstream};

  include /etc/nginx/nginx-wp-common.conf;
}
```

Every time we add a new site to `vvv-custom.yml` or the provisioner files, we need to reprovision VVV. To do this, you need to run

`vagrant reload --provision`

After that, either import the database via phpMyAdmin or `WP-CLI`, or start from scratch.

### An alternative way of adding sites

Another way you can add new sites is by using the `custom site template`. In the `vvv-custom.yml` file add

```yaml
sites:

  .... other sites...

  example:
    repo: https://github.com/Varying-Vagrant-Vagrants/custom-site-template.git
    hosts:
      - example.test
```

Then save `vvv-custom.yml` and run `vagrant reload --provision` to update VVV with the new site. Always reprovision after making changes to `vvv-custom.yml`. Be sure to indent correctly as whitespace matters in YAML files. VVV prefers to indent using two spaces.
