There are several ways of setting up your dev environment on Windows OS. The one we opted to showcase here is using the Windows Subsystem for Linux (we'll use WSL2 or just WSL through this document).
Under the hood is a plain Ubuntu 20.04, so you should be able to set up the entire LEMP stack without a problem.

There are two ways you can try to set up the dev environment on WSL, one is more foolproof, but takes a bit longer, the other is by just trying to set up [valet-wsl](https://github.com/valeryan/valet-wsl). It is faster, but we've had issues with it in the past.

## Installing the required tools

### WSL2

Make sure you have either Windows 10 or 11 installed.

For Windows 10, make sure you have:

- For x64 systems: version 1903 or higher, with build 18362 or higher
- For ARM64 systems: version 2004 or higher, with build 19041 or higher
- Builds lower than 18362 do not support WSL2, please update your Windows.

To check the version and build number, either open up a start menu and in the search bar, search for: `winver`, or open a run window (Windows key + R) and type `winver`.

Make sure you have virtualization enabled in your BIOS, otherwise WSL won't work as intended.

**Note**

In some cases, you might have to enable certain Windows features on. Most notably you should have `Virtual Machine Platform` and `Windows Subsystem for Linux` enabled in the Windows features list (they are located in the Control Panel -> Programs -> Turn Windows features on or off link).

---

Open the PowerShell or Windows Command Prompt **as an administrator** and type

```bash
wsl --install
```

This command will enable the required optional components, download the latest Linux kernel, set WSL 2 as your default, and install a Linux distribution for you (Ubuntu by default).

The first time you launch a newly installed Linux distribution, a console window will open, and you'll be asked to wait for files to de-compress and be stored on your machine. All future launches should take less than a second.

In the case there was some issue you can check the default WSL version with

```bash
wsl -l -v
```

and if it's not set to 2, set it using the following command:

```bash
wsl --set-default-action 2
```

In the case WSL is already installed you can check the list of available distros with `wsl --list --online` and you can run

```bash
wsl --install -d <DistroName>
```

to install a desired Linux distro.

For more information on the WSL installation you can check the [following link](https://docs.microsoft.com/en-us/windows/wsl/install).

### Terminal

Windows has the new Windows Terminal app that you can download from the [Windows Store](https://apps.microsoft.com/store/detail/9N0DX20HK701). Once WSL is installed, it should show up in the Terminal app.

Once you've set up the terminal, follow the [Git Setup](./Git Setup.md) chapter, to set Git up on your system.

### PHP

To have more flexibility in choosing your PHP version we are going to use `[phpenv](https://github.com/phpenv/phpenv)`. In your terminal app type the following:

```bash
curl -L <https://raw.githubusercontent.com/phpenv/phpenv-installer/master/bin/phpenv-installer> | bash

sudo apt update && sudo apt install -y build-essential re2c libxml2-dev libssl-dev libsqlite3-dev bison pkg-config libbz2-dev libcurl4-gnutls-dev libjpeg-dev libpng-dev libmcrypt-dev libtidy-dev libxslt1-dev zlib1g-dev libcurl4-openssl-dev libonig-dev php-imagick libedit-dev libreadline-dev libxslt-dev libzip-dev autoconf

phpenv install 7.4.30
phpenv global 7.4.30
```

The `sudo apt install` part will install all the necessary libs for the PHP to work.

After the installation, you'll need to check the user and group in the `~/.phpenv/versions/$VERSION/etc/php-fpm.d/*.conf` and change it to your current user (which you can check using `whoami` command).

### Composer

Once you have php set up, you can install Composer using [phpenv-composer](https://github.com/ngyuki/phpenv-composer).

```bash
cd ~/.phpenv/plugins
git clone <https://github.com/ngyuki/phpenv-composer.git>
phpenv rehash
phpenv global 7.4.30
composer --version
```

You should see the latest Composer version outputted.

### Nginx

To install and set up Nginx type the following commands in your terminal:

```bash
sudo apt-get -y install curl gnupg2 ca-certificates lsb-release ubuntu-keyring
curl <https://nginx.org/keys/nginx_signing.key> | gpg --dearmor \\
    | sudo tee /usr/share/keyrings/nginx-archive-keyring.gpg >/dev/null
gpg --dry-run --quiet --import --import-options import-show /usr/share/keyrings/nginx-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/nginx-archive-keyring.gpg] \\
<http://nginx.org/packages/ubuntu> `lsb_release -cs` nginx" \\
    | sudo tee /etc/apt/sources.list.d/nginx.list
echo -e "Package: *\\nPin: origin nginx.org\\nPin: release o=nginx\\nPin-Priority: 900\\n" \\
    | sudo tee /etc/apt/preferences.d/99nginx

sudo apt-get install -y nginx
```

In order to set up your webserver, you should use nginx config. You can find good config [here](https://github.com/h5bp/server-configs-nginx).

```bash
nginx -s stop
cd /etc
mv nginx nginx-previous
git clone <https://github.com/h5bp/server-configs-nginx.git> nginx
# install-specific edits
nginx
```

You can modify the default template, located in `/etc/nginx/conf.d/templates/example.com.conf` to look like this:

```bash
# ----------------------------------------------------------------------
# | Config file for actual-hostname host                                   |
# ----------------------------------------------------------------------
#
# This file is a template for an Nginx server.
# This Nginx server listens for the `actual-hostname` host and handles requests.
# Replace `actual-hostname` with your hostname before enabling.

# Choose between www and non-www, listen on the wrong one and redirect to
# the right one.
# <https://www.nginx.com/resources/wiki/start/topics/tutorials/config_pitfalls/#server-name-if>
server {
  listen [::]:443 ssl http2;
  listen 443 ssl http2;

  server_name www.example.test;

  include h5bp/tls/ssl_engine.conf;
  include h5bp/tls/certificate_files.conf;
  include h5bp/tls/policy_balanced.conf;

  return 301 $scheme://example.test$request_uri;
}

server {
  # listen [::]:443 ssl http2 accept_filter=dataready;  # for FreeBSD
  # listen 443 ssl http2 accept_filter=dataready;  # for FreeBSD
  listen [::]:443 ssl http2;
  listen 443 ssl http2;

  # The host name to respond to
  server_name example.test;

  include h5bp/tls/ssl_engine.conf;
  include h5bp/tls/certificate_files.conf;
  include h5bp/tls/policy_balanced.conf;

  # Path for static files
  root /home/YOURUSERNAME/Sites/example;

  # Custom error pages
  include h5bp/errors/custom_errors.conf;

  # Include the basic h5bp config set
  include h5bp/basic.conf;

  # Favicon notices
  location = /favicon.ico {
    log_not_found off;
    access_log off;
  }

  location = /robots.txt {
    allow all;
    log_not_found off;
    access_log off;
  }

  location / {
    # URLs to attempt, including pretty ones.
    try_files $uri $uri/ /index.php?$query_string;
  }

  location ~ \\.php$ {
    #NOTE: You should have "cgi.fix_pathinfo = 0;" in php.ini
    try_files                $uri =404;
    fastcgi_split_path_info  ^(.+\\.php)(/.+)$;
    fastcgi_pass             127.0.0.1:9000;
    fastcgi_index            index.php;
    fastcgi_intercept_errors on;
    fastcgi_param            SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include                  fastcgi_params;
  }
}
```

Make sure you replace the `YOURUSERNAME` with your actual username. From the config file, you can see that all the sites are located in the `/home/YOURUSERNAME/Sites/` folder.

The above template will be used to set up a new site.

Whenever you are making changes to your Nginx config, you can test it using sudo `nginx -t` command. If you make an error, the test command will point it out.

### MySQL

You can install MySQL 8 by typing the following:

```bash
sudo apt-get install -y mysql-server
sudo /etc/init.d/mysql start
sudo mysql_secure_installation
```

After setup just change the root password by logging in

```bash
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '';
```

Since it's for local development, you don't need to have a password set up.

### NodeJS

To install and use NodeJS, you can use [nvm](https://github.com/nvm-sh/nvm):

```bash
curl -o- <https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh> | bash
```

or

```bash
wget -qO- <https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh> | bash
```

if `curl` isn't available.

After nvm has been installed, you may need to run `exec $SHELL` (it's a kin to 'restarting' your current shell, so any additions are recognized). Then you can install and use the LTS version of node:

```bash
nvm install 16

nvm use 16
```

Node v16 is the current (at the time of writing) the latest stable version (LTS), so we should always use it.

### WP-CLI

WP-CLI is our bread and butter, so you should install it on your system. You can do it by typing the following commands in your terminal:

```bash
curl -O <https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar>
php wp-cli.phar --info
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp
```

### Adding sites

In order to quickly be able to add the sites you can add the following functions in your `.bashrc` or `.zshrc` file:

```bash
# WP-CLI site setup
# Usage create-site {folder name} {database name} {site title}
create-site () {
  mkdir $1 && cd $1
  wp core download --skip-content;
  wp config create --dbname=$2 --dbuser=root;
  wp config set DB_HOST "127.0.0.1"
  mkdir wp-content && mkdir wp-content/themes && mkdir wp-content/plugins;
  (
      wp db create || true
      wp core install --url="$1"".test" --title=$3 --admin_user=admin --admin_password=password --admin_email=admin@sitetitle.com
  )
  cd /etc/nginx/conf.d
  sudo cp templates/example.com.conf .$1.test.conf
  sudo sed -i "s/example/$1/g" .$1.test.conf
  sudo mv .$1.test.conf $1.test.conf
  make-certificate $1.test
  restart-server
  cd "$HOME/Sites/$1"
  # ADD SITE TO etc/hosts on Windows: C:\\Windows\\System32\\drivers\\etc\\hosts !!!!
}

# Restart server
restart-server() {
  PHP_VERSION=$(php -r 'echo PHP_VERSION;')
  # Restart mysql
  sudo service mysql restart
  # Restart nginx
  sudo service nginx restart
  # Restart PHP-FPM
  sudo ~/.phpenv/versions/$PHP_VERSION/etc/init.d/php-fpm restart
  # Restart/Start Mailhog - optional, if you have Mailhog installed
  sudo service mailhog restart
}

# Generate a self-signed SSL certificate
# Usage make-certificate site-name
make-certificate() {
  cd /etc/nginx
  if [ ! -d "/etc/nginx/certs" ]
  then
      sudo mkdir /etc/nginx/certs
  fi
  sudo openssl req -newkey rsa\\:2048 -nodes -keyout /etc/nginx/certs/$1.key -x509 -days 365 -out /etc/nginx/certs/$1.crt
}
```

Once you've added it, don't forget to run `exec $SHELL` to 'restart' your shell.

Then, in your `~/Sites` folder you can just type:

```bash
create-site infinum-academy infinum_academy "Infinum Academy WordPress Project"
```

This command will:

1. Create a directory called `infinum-academy`
2. Download WordPress core
3. Create wp-config.php file
4. Create a database called `infinum_academy`
5. Install WordPress that will point to `infinum-academy.test` URL
6. Setup Nginx template
7. Create SSL certificate for the site (Just fill in the details, it's self-signed certificate)
8. Restart the local server so that everything is connected

Last piece of puzzle to do is to go to the `C:\\Windows\\System32\\drivers\\etc\\hosts` file, and add the site to the hosts file

```bash
127.0.0.1 infinum-academy.test
```

This part cannot be automated from the terminal, because you'd have to allow terminal to access the Windows system, and that's a security risk.

## Troubleshooting

Because WSL is a subsystem, there are some things you need to keep an eye out for, but we'll get to those in a little while.

### Exposing hosts to your browser

In order to have the URLs exposed to your browser you need to modify your hosts file.
You should open the notepad program with administrator access and open the `C:\\Windows\\System32\\drivers\\etc\\hosts` file.

Then, at the end of the file, you should just manually add

```bash
127.0.0.1 yoururlgoeshere.test
```

### Changing PHP versions

When you have phpenv changing the PHP version is super easy. Say we want to install PHP 8.1. You'd run

```bash
phpenv install 8.1.0
phpenv global 8.1.0
```

You should use the `restart-server` function to restart the webserver. Sometimes though, that may not work, so you might need to restart the WSL.

### Internet is slow from WSL

Sometimes that happens in WSL. You can try to restart the wsl from the Powershell terminal by using `wsl --shutdown` and restarting it again.

You may need to add the in your `/etc/wsl.conf` the following:

```bash
[network]
generateResolvConf = false
```

And in your `/etc/resolv.conf` (create it if it doesn't exist) add:

```bash
nameserver 8.8.8.8 # Or use your DNS server instead of 8.8.8.8 which is a Google DNS server
```

Make sure you change the attributes of the resolv file

```bash
sudo chattr +i resolv.conf
```

Then in the Powershell run `wsl --shutdown` and restart the WSL2.

## Useful links

[https://linuxize.com/post/using-the-ssh-config-file/](https://linuxize.com/post/using-the-ssh-config-file/)[https://madebydenis.com/my-development-setup/](https://madebydenis.com/my-development-setup/)
**Note**
