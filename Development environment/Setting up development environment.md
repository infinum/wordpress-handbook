When you want to develop a WordPress app, or you are joining an existing project, it is important to have local development ready. There are a number of solutions that can be used.

If you want to use simple solutions that only include the bare minimum (server config), you can use [WAMP](http://www.wampserver.com/en/), [MAMP](https://www.mamp.info/en/), [XAMPP](https://www.apachefriends.org/index.html), or [Local by Flywheel](https://local.getflywheel.com/).

However, we do recommend more WordPress-friendly solutions. One such solution is [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV). It is a Vagrant configuration focused on WordPress development. The advantage of VVV over MAMP or XAMPP is that it comes bundled with a WordPress-ready server configuration and software packages, such as `php-fpm`, `WP-CLI`, `Composer`, `NodeJs`, and other useful packages.

The advantage of using Vagrant is that it is configured independently of your machine because it runs on a virtual machine. In this way, everyone on your team will develop in the same environment.

[WP Docker](https://10up.com/blog/2017/wp-docker/) is another development environment that you can use.

The main difference is that Vagrant uses virtual machines to run environments independent of the host machine via [VirtualBox](https://www.virtualbox.org/) or [VMware](http://www.vmware.com/). Each environment has its own virtual machine. Docker, on the other hand, uses 'containers' that include your application and all of its dependencies, but shares the operating system with other containers. They are isolated processes on the host operating system, but are not tied to any specific infrastructure.

**Vagrant** is easier to set up and get up and running, but can be resource-intensive (RAM and space on the disk).
**Docker** is a bit harder to understand and set up, but it is much faster and uses less CPU and RAM than Vagrant.

In the following chapters, you'll learn how to set up various environments.

