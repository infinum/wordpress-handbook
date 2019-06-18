Homestead is an official, pre-packaged Vagrant box from Laravel team. Its default approach is having multiple sites set up on a single Vagrant instance, which makes adding new sites a trivial task. For more advanced requirements there is an option to set up each site on its own instance.

Homestead runs on any Windows, Mac, or Linux system, and includes the Nginx web server, PHP 7.3, PHP 7.2, PHP 7.1, MySQL, PostgreSQL, Redis, Memcached, Node, etc.. For full list of features check out the [offical documentation](https://laravel.com/docs/5.8/homestead)

## Installing Laravel Homestead

As any other Vagrant boxes, Homestead requires a few prerequisites installed:

* [Vagrant](https://www.vagrantup.com/downloads.html)
* [Git](https://git-scm.com/downloads)
* Either one of the following: [VirtualBox](https://www.virtualbox.org/wiki/Downloads), [VMWare](https://my.vmware.com/en/web/vmware/downloads)<sup>1</sup>, [Parallels](https://www.parallels.com/products/desktop/)<sup>2</sup>, or [Hyper-V](https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

<sup>1 Requires the purchase of both VMware Fusion / Workstation and the VMware Vagrant plug-in.</sup>

<sup>2 Not free, but has a free trial period and requires a free Vagrant plug-in</sup>

For easiest setup use [VirtualBox](https://www.virtualbox.org/wiki/Downloads) as it is free, multi-platform and regularly updated.

After everything is installed we can begin with Homestead setup.

First add/download the `laravel/homestead` box to your Vagrant installation from terminal:

```sh
vagrant box add laravel/homestead
```

Clone the Homestead repository in a directory that will hold all your Homestead projects:

```sh
git clone https://github.com/laravel/homestead.git ~/homestead
```

Go into the directory that repo was cloned in and checkout the latest stable release tag:

```sh
cd ~/homestead

# Checkout 8.4.0, or a newer stable tag.
git checkout v8.4.0
```

Next up, run `init.sh` or `init.bat` depending on the system you are using. This will create `Homestead.yml` file that will be used for configuring the environment:

```sh
# Mac OSX and Linux
bash init.sh

# Windows
init.bat
```

Now that we have `Homestead.yml` we can start configuring our environment.

## Configuring Homestead

Open `Homestead.yml` in your code editor. You'll see that some parts are already filled.

First thing that needs to be configured is `provider`. This relates to the virtualization provider that was installed.

```yml
# Default value
provider: virtualbox

# Other providers
provider: vmware_fusion | vmware_workstation | parallels | hyperv
```

One of the most important things to configure is shared folder config. These folders will contain all files for our websites. Shared folders are defined like:

```yml
folders:
    # Local path to the directory in which we cloned Homestead repo in
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

Other important thing to configure is sites. They are stored in one or many shared folders and they are defined with domain name that will map to a site folder:

```yml
sites:
    # Local domain name
    - map: homestead.test
      # Location of the sites public folder
      to: /home/vagrant/homestead/www/
  ```

Sites domain name must also be mapped to the actual IP address of the Vagrant server. That is done by editing `hosts` file of the local machine. This can be done manually, but for a more automated approach it is recommended to use [vagrant-hostsupdater](https://github.com/cogitatio/vagrant-hostsupdater) plugin.

Install it by running: `vagrant plugin install vagrant-hostsupdater` inside the terminal while in `/homestead` directory. Homestead already has config for `vagrant-hostsupdater` plugin and should map all sites domains to Vagrants IP address.
By default IP address is defined in `Homestead.yml` but you can change it to some other address.

Note for Windows users: You'll need to change permissions on `hosts` file to allow current user write access to that file.

Last thing for basic configuration is database property. With this we define all databases that will be created on server startup or reprovision.
```yml
databases:
  - homestead
  - wordpress
  ```

Other thing to note is that default Homestead database name is `homestead` and password is `secret`.

## Using Homestead

When we are done with Homestead configuration, we can start the server with command `vagrant up`, stop the server with `vagrant halt`, restart it with `vagrant reload`. If something is changed in `Homestead.yml` configuration server will need to be reprovisioned so that all changes in configuration are reflected on our sites. To reprovision a server use `vagrant reload --provision`.

Now you have everything to run your WordPress site. Download the instance of WordPress in your preferred way and set it up in `wp-config.php`. To finish installation go to your local site URL (eg. `wordpress.test`).

## Extra features

### Mailhog

Mailhog is really useful tool for intercepting incoming and outgoing emails on local machine. To set it up first create `.env` file in `/homestead` directory and place Mailhog config inside:
```
MAIL_DRIVER=smtp
MAIL_HOST=localhost
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```
After that Mailhog interface is accessible on `http://localhost:8025`

### Accessing Vagrant from inside

Sometimes you'll need to change some things inside the virtual machine eg. run a database export or edit `php.ini` file. To access it type `vagrant ssh` and you are in.

### Sharing your sites

Vagrant offers a way to temporarily make your local site publicly accessible. That can work with command `vagrant share`, but will only work if only one site is set up for that Vagrant instance.
Homestead has its own implementation of that feature that supports sharing of multiple sites. It uses Ngrok to tunnel to your local machine.
To do this first access your virtual machine with `vagrant ssh` and then run `share sitename.domain`. Ngrok will do its thing and provide you with publicly accessible URLs for you local site.

For all other advanced feature please check out the [official documentation](https://laravel.com/docs/homestead)
