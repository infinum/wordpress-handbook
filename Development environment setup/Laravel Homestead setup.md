Homestead is an official, pre-packaged Vagrant box from the Laravel team. Its default approach is to have multiple sites set up on a single Vagrant instance, which makes adding new sites a trivial task. For more advanced requirements, each site can also be set up on its own instance.

Homestead runs on any Windows, Mac, or Linux system and includes the Nginx web server, PHP 7.3, PHP 7.2, PHP 7.1, MySQL, PostgreSQL, Redis, Memcached, Node, etc. For a full list of features, check out the [official documentation](https://laravel.com/docs/8.x/homestead)

## Installing Laravel Homestead

As any other Vagrant box, Homestead requires a few prerequisites installed:

* [Vagrant](https://www.vagrantup.com/downloads.html)
* [Git](https://git-scm.com/downloads)
* Either one of the following: [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [VMWare](https://my.vmware.com/en/web/vmware/downloads)<sup>1</sup>, [Parallels](https://www.parallels.com/products/desktop/)<sup>2</sup>, or [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

<sup>1 Requires the purchase of both VMware Fusion / Workstation and the VMware Vagrant plug-in.</sup>

<sup>2 Not free but has a free trial period and requires a free Vagrant plug-in.</sup>

<sup>If you want to use one of the paid options, you can use your education budget to cover the cost. For more information, consult your team lead.</sup>

For the easiest setup, use [VirtualBox](https://www.virtualbox.org/wiki/Downloads) — it is free, multi-platform, and regularly updated.

After everything is installed, we can begin with Homestead setup.

First, add/download the `laravel/homestead` box to your Vagrant installation from the terminal:

```bash
vagrant box add laravel/homestead
```

Clone the Homestead repository into a directory that will hold all your Homestead projects:

```bash
git clone https://github.com/laravel/homestead.git ~/homestead
```

Go into the directory the repo was cloned into, and check out the latest stable release tag:

```bash
cd ~/homestead

# Checkout latest stable tag.
# At the time of writing v8.4.0 is latest stable version
# Check the latest version on https://github.com/laravel/homestead/releases
git checkout v8.4.0
```

Next, run `init.sh` or `init.bat` depending on the system you are using. This will create a `Homestead.yml` file that will be used to configure the environment:

```bash
# Mac OSX and Linux
bash init.sh

# Windows
init.bat
```

Now that we have `Homestead.yml`, we can start configuring our environment.

## Configuring Homestead

Open `Homestead.yml` in your code editor. You'll see that some parts are already filled.

Provider is the first thing that needs to be configured. This relates to the virtualization provider that was installed.

```yml
# Default value
provider: virtualbox
# Other providers
provider: vmware_fusion | vmware_workstation | parallels | hyperv
```

One of the most important things to configure is the shared folder config. These folders will contain all files for our websites. Shared folders are defined like this:

```yml
folders:
    # Local path to the directory where we cloned the Homestead repo
    - map: ~/homestead
      # Path to the directory from the virtual machine
      to: /home/vagrant/homestead
```

For multiple sites, each with its own instance:

```yml
folders:
    - map: ~/homestead/project-1
      to: /home/vagrant/homestead/project-1

    - map: ~/homestead/project-2
      to: /home/vagrant/homestead/project-2
```

It is also important to configure sites. They are stored in one or many shared folders, and they are defined with a domain name that will map to a site folder:

```yml
sites:
    # Local domain name
    - map: homestead.test
      # Location of the sites public folder
      to: /home/vagrant/homestead/www/
  ```

The site's domain name must also be mapped to the actual IP address of the Vagrant server. This is done by editing the `hosts` file of the local machine. You can do this manually. However, for a more automated approach, it is recommended to use the [vagrant-hostsupdater](https://github.com/cogitatio/vagrant-hostsupdater) plugin.

Install it by running `vagrant plugin install vagrant-hostsupdater` inside the terminal while in the `/homestead` directory. Homestead already has a config for the `vagrant-hostsupdater` plugin and should map all site's domains to the Vagrant's IP address.
The IP address is by default defined in `Homestead.yml`, but you can change it to some other address.

Note for Windows users: You'll need to change permissions on the `hosts` file to give the current user write access to that file.

The last thing in the basic configuration is database property. With this, we define all databases that will be created on server startup or reprovision.

```yml
databases:
  - homestead
  - wordpress
  ```

Another thing to note is that the default Homestead database name is `homestead`, and the password is `secret`.

## Using Homestead

When we are done with the Homestead configuration, we can start the server with the `vagrant up` command, stop the server with `vagrant halt`, and restart it with `vagrant reload`. If something is changed in `Homestead.yml`, the configuration server will need to be reprovisioned so that all changes in the configuration are visible on our sites. Use `vagrant reload --provision` to reprovision a server.

Now you have everything you need to run your WordPress site. Download the instance of WordPress in your preferred way and set it up in `wp-config.php`. To finish the installation, go to your local site URL (e.g., `wordpress.test`).

## Extra features

### Accessing Vagrant from inside

Sometimes, you'll need to change some things inside the virtual machine, e.g. access your MySQL database, edit the `php.ini` file, or run some WP CLI functions. To access it, type `vagrant ssh`, and you are in. Since the server and database are inside the virtual machine, this is the only way to access them.

You will be positioned in the `/home` directory that will contain only the `/homestead` directory (the same one that holds the Homestead configuration and sites). From there, you can navigate to the root folder and other parts of the Linux installation.

### Installing PHPMyAdmin

To install PHPMyAdmin on Homestead you will need to setup a new site in `Homestead.yml` for a specific Homestead instance. Define a local URL to PHPMyAdmin application and

```yml
sites:
    - map: phpmyadmin.test
      to: /home/vagrant/homestead/www/phpmyadmin
```

After that, log into the homestead box with `vagrant ssh` and position yourself in a directory that holds all your projects (in this case that would be the `www` directory).

Run `curl -sS https://raw.githubusercontent.com/grrnikos/pma/master/pma.sh | sh` and that should be it. Visit the PHPMyAdmin local URL and log in with the default Homestead database credentials.

### Mailhog

Mailhog is a really useful tool for intercepting incoming and outgoing emails on a local machine. To set it up, first create a `.env` file in the `/homestead` directory and place the Mailhog config inside:

```
MAIL_DRIVER=smtp
MAIL_HOST=localhost
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```
After that, the Mailhog interface will be accessible at `http://localhost:8025`.

### Sharing your sites

Vagrant offers a way to temporarily make your local site publicly accessible. That can be done with the `vagrant share` command, but it will work only if only one site is set up for that Vagrant instance.
Homestead has its own implementation of that feature that supports sharing multiple sites. It uses Ngrok to tunnel to your local machine.

To do this, first access your virtual machine with `vagrant ssh` and then run `share sitename.domain`. Ngrok will do its thing and provide you with the publicly accessible URLs for you local site.

For other advanced features, please check out the [official documentation](https://laravel.com/docs/homestead).
