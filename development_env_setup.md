# Setting up development environment

When you want to develop a WordPress app, or you are joining a development of an exisiting project, it is important that you have a local development ready. There are number of solutions that can be used.

If you want to use simple solutions that only include bare minimum (server config), you can use [WAMP](www.wampserver.com/en/), [MAMP](https://www.mamp.info/en/) or [XAMPP](https://www.apachefriends.org/index.html).

However, we do recommend a more WordPress friendly solutions. One such solution is [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV). It is a Vagrant configuration focused on WordPress development. The advantage of VVV over MAMP or XAMPP is that it comes bundled with WordPress ready server configuration and software packages such as `php-fpm`, `WP-CLI`, `Composer`, `NodeJs` and other useful packages.

The advantage of using Vagrant is that it is configured independently of your machine, because it runs on a virtual machine. That way everyone on your team will develop on the same environment.

Another development environment that you can use is [WP Docker](https://10up.com/blog/2017/wp-docker/).

The main difference is that Vagrant uses virtual machines to run environments independent of the host machine via [VirtualBox](https://www.virtualbox.org/) or [VMware](http://www.vmware.com/). Each environment has its own virtual machine. Docker, on the other hand, uses 'containers' that include your application and all of its dependencies, but shares the operating system with other containers. They are isolated processes on the host operating system, but are not tied to any specific infrastructure.

**Vagrant** is easier to set up and get up and running, but can be resource intensive (RAM and space used on the disk).
**Docker** is a bit harder to understand and set up, but is much faster and uses less CPU and RAM than Vagrant.

In the following chapters you'll see how to set up various environments.

