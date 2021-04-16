## Installing VVV

Setting up VVV is easy. You can either follow the manual install instructions, as described in the [official documentation](https://varyingvagrantvagrants.org/docs/en-US/), or you can use the `brew` package manager for quick and easy installation.

First, install [VirtualBox](https://www.virtualbox.org/wiki/Downloads). You can install it using Homebrew:

`brew cask install virtualbox`

After you install VirtualBox, install [Vagrant](https://www.vagrantup.com/downloads.html).

`brew cask install vagrant`

Then you'll want to install a few Vagrant plugins.

```bash
vagrant plugin install vagrant-hostsupdater
```

We recommend that you reboot your computer to avoid any networking issues.

After you install the plugins, you'll want to install VVV in the local folder.

```bash
cd ~
git clone -b stable git://github.com/Varying-Vagrant-Vagrants/VVV.git ~/vagrant-local
cd vagrant-local
```

This will clone the official VVV repository to your `vagrant-local` folder in the home folder. If you want the latest updates and features, switch to the `develop` branch instead of the `master` branch.

Before starting VVV, go to `Vagrantfile` and uncomment the `config.vm.network :public_network` line. This will enable you to debug across devices later on.

You can set up your custom sites by creating a copy of the `default-config.yml` file and renaming it to `custom.yml`. Both are located in the `config` folder.

While in your `vagrant-local` folder, type

`vagrant up`

This will set VVV up for the first time. This may take some time (about 10 minutes on an average network). While VVV is setting up, you can read the rest of this handbook.

What Vagrant is actually doing is downloading a packaged box with the Ubuntu virtual machine and caching it for future use. After downloading, it will provision the running script that will download other packages necessary for local development.

Once it has installed everything, you'll be all set to work on your local WordPress. You can type

`http://vvv.test`

in your browser, which will open a screen with some interesting links you can explore.

![vvv.test screen](/img/vagrant.png)

When you want to close Vagrant and save your RAM, type

`vagrant halt`

The next time you start your Vagrant with `vagrant up`, the cached box will start and boot in a minute or so. Re-provisioning your Vagrant is necessary when adding new folders or changing your configuration.

## Making your Vagrant public-friendly

Modern web development is *mobile first* oriented. With that in mind, it is natural that you want to be able to see what you are developing on your mobile phone.

To do that, you need to change your `Vagrantfile` located in the `vagrant-local` folder. Search for

`config.vm.network :public_network`

and uncomment it. You can also enable port forwarding while you're at it:

`config.vm.network "forwarded_port", guest: 80, host: 8888`

Now you need to re-provision your Vagrant.

`vagrant reload --provision`

When you do that, you'll be asked which network interface you use to connect to the Internet.

```bash
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

By using it, you'll be able to use [BrowserSync](https://www.npmjs.com/package/browser-sync-webpack-plugin) to test the development in your browser and on a mobile phone. Follow the instructions in the [Eightshift docs](https://infinum.github.io/eightshift-docs/), and you should be able to easily inspect your site without much hassle.

## Adding new sites

The official documentation on adding a new site can be found [here](https://varyingvagrantvagrants.org/docs/en-US/adding-a-new-site/). An easier way to provision a new site is by using [site templates](https://varyingvagrantvagrants.org/docs/en-US/site-templates/).
A site template is a Git repo that contains scripts and files necessary to set up a new VVV site.

You can also create provision scripts. First, you need to add your site to the `custom.yml` file.

Let's say we want to create a `wordpress-infinum` site. After the

```yaml
  wordpress-two:
    skip_provisioning: false
    description: "A standard WP install, useful for building plugins, testing things, etc"
    repo: https://github.com/Varying-Vagrant-Vagrants/custom-site-template.git
    custom:
      # locale: it_IT
      delete_default_plugins: true
      install_plugins:
        - query-monitor
    hosts:
      - two.wordpress.test
```

code in the file add

```yaml
  wordpress-infinum:
    repo: https://github.com/Varying-Vagrant-Vagrants/custom-site-template.git
    vm_dir: /srv/www/infinum/wordpress-infinum
    local_dir: www/infinum/wordpress-infinum
    hosts:
      - wordpress-infinum.test
```

We've told Vagrant that there should be a site it can access inside `www/infinum/wordpress-infinum`. So we need to create it.

Every time we add a new site to `custom.yml` or the provisioner files, we need to reprovision VVV. To do this, you need to run

`vagrant reload --provision`

After that, either import the database via phpMyAdmin or `WP-CLI` or start from scratch.

You can modify the provision scripts if you need a special setup.
