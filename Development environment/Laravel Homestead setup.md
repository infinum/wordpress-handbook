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

### Configuring Homestead

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

TBC ...
