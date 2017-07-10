# Varying Vagrant Vagrants setup

## Installing VVV

Setting up VVV is easy. You can either follow the manuall install, as described in the [official documentation](https://varyingvagrantvagrants.org/docs/en-US/), or you can use `brew` package manager for quick and easy installation.

First install VirtualBox

`brew cask install virtualbox`

After VirtualBox installs install VMWare

`brew cask install vagrant`

Then you'll want to install few Vagrant plugins

```sh
vagrant plugin install vagrant-hostsupdater
vagrant plugin install vagrant-triggers
```

After you install plugins, you'll want to install the VVV in the local folder

```sh
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

Next time you start your Vagrant with `vagrant up` the cached box will start and it will boot in a minute or so. Reprovisioning your Vagrant is only necessary when adding new folders or changing your Vagrant configuration, which will be described below.

## Making your vagrant public aware

Modern web development is *mobile first* oriented. With that in mind, it is natural that you'd want to be able to see what you are developing locally on your mobile phone.

To do that, you need to change your `Vagrantfile` that is located in the `vagrant-local` folder. Search for

`config.vm.network :public_network`

And uncomment it. You can also enable port forwarding while you're at it

`config.vm.network "forwarded_port", guest: 80, host: 8888`

Now you need to reprovision your Vagrant

`vagrant reload --provision`

When you do that you'll be asked to which network interface you use to connect to the internet

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

In our case we connect via Wi-Fi, so choose 1. If you're connecting via ethernet, you'll need to select that as your network bridge.

There are two ways to enable inspecting your theme on mobile

### xip.io service

Once your vagrant finishes provisioning your can ssh to it

`vagrant ssh`

Type `ifconfig`, you should see something like this

```sh
vagrant@vvv:~$ ifconfig
eth0      Link encap:Ethernet  HWaddr 08:00:27:36:92:90
          inet addr:10.0.2.15  Bcast:10.0.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe36:9290/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:13405 errors:0 dropped:0 overruns:0 frame:0
          TX packets:4884 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:12428370 (12.4 MB)  TX bytes:640262 (640.2 KB)

eth1      Link encap:Ethernet  HWaddr 08:00:27:eb:20:58
          inet addr:192.168.50.4  Bcast:192.168.50.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:feeb:2058/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:7 errors:0 dropped:0 overruns:0 frame:0
          TX packets:17 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:602 (602.0 B)  TX bytes:1526 (1.5 KB)

eth2      Link encap:Ethernet  HWaddr 08:00:27:17:ec:a3
          inet addr:192.168.2.167  Bcast:192.168.2.255  Mask:255.255.255.0
          inet6 addr: fe80::a00:27ff:fe17:eca3/64 Scope:Link
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:3066 errors:0 dropped:2 overruns:0 frame:0
          TX packets:26 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:360118 (360.1 KB)  TX bytes:3524 (3.5 KB)

lo        Link encap:Local Loopback
          inet addr:127.0.0.1  Mask:255.0.0.0
          inet6 addr: ::1/128 Scope:Host
          UP LOOPBACK RUNNING  MTU:65536  Metric:1
          RX packets:10 errors:0 dropped:0 overruns:0 frame:0
          TX packets:10 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:1230 (1.2 KB)  TX bytes:1230 (1.2 KB)
```

From this list we can figure out what IP address we can now use to access our local development from our mobile phones.

Vagrant's default IP address (that you've seen in the Vagrant file) is `192.168.50.4`, and `127.0.0.1` points to home. So those two are out. That leaves you with `10.0.2.15` and `192.168.2.167`. Now if you check your laptop's IP address, you'll see that it starts with something like: `192.168.x.x`, so the second one is usually the one you want

Type that IP address in your mobile browser and you should see this:

![vvv.dev screen on mobile](https://github.com/dingo-d/wordpress-handbook/blob/master/images/mobile-vagrant.png)

So how can we access our project via mobile? We use the name of the site we defined, for example say we have `wordpress-infinum.dev` site, instead of `.dev` add IP address and suffix `xip.io`

`wordpress-infinum.192.168.2.167.xip.io`

### Using webpack and BrowserSyncPlugin

At infinum we use [wp-boilerplate](https://github.com/infinum/wp-boilerplate) to kickstart our development. It is a modern way that uses [webpack](https://webpack.js.org/) to bundle your assets.

By using that you'll be able to use [BrowserSync](https://www.npmjs.com/package/browser-sync-webpack-plugin) to test the development on your browser and mobile phone. Jus follow the instructions in the wp-boilerplate repo, and you should be able to easily inspect your site without much hassle.

## Adding new sites

There are few things you need to do when setting up a new site. First, you need to add your site to the `vvv-custom.yml` file. It is just a modified copy of the existing `vvv-config.yml` file.

Let's say we want to create `wordpress-infinum` site. After the

```yaml
wordpress-develop:
    repo: https://github.com/Varying-Vagrant-Vagrants/vvv-wordpress-develop.git
    hosts:
      - src.wordpress-develop.dev
      - build.wordpress-develop.dev
```

code in the file add

```yaml
wordpress-infinum:
    hosts:
      - wordpress-infinum.dev
```

We've told Vagrant that inside `www/wordpress-infinum` there should be a site it can access. So we need to create it. Inside that folder we need to create two additional folders: `provision` and `public_html`.

`provision` folder holds the scripts that will set up the database and nginx configuration. `public_html` folder holds the WordPress installation.

First file that you'll add to the `provision` folder will be `vvv-init.sh`

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

```sh
server {
  listen 80;
  listen 443 ssl;
  server_name wordpress-infinum.dev ~^wordpress\-infinum\.\d+\.\d+\.\d+\.\d+\.xip\.io$;
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

