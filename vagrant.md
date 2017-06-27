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
``

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

## Adding new