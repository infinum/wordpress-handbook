When you want to develop a WordPress app, or you are joining an existing project, it is important to have local development ready. There are a number of solutions that can be used.

### App based dev solutions

If you want to use simple solutions that only include the bare minimum (server config), you can use [WAMP](http://www.wampserver.com/en/), [MAMP](https://www.mamp.info/en/), [XAMPP](https://www.apachefriends.org/index.html), or [Local by Flywheel](https://local.getflywheel.com/).

### Vagrant based  dev environments

We do recommend more WordPress-friendly solutions. One such solution is [Varying Vagrant Vagrants](https://github.com/Varying-Vagrant-Vagrants/VVV). It is a Vagrant configuration focused on WordPress development. The advantage of VVV over MAMP or XAMPP is that it comes bundled with a WordPress-ready server configuration and software packages, such as `php-fpm`, `WP-CLI`, `Composer`, `NodeJs`, and other useful packages.

The advantage of using Vagrant is that it is configured independently of your machine because it runs on a virtual machine. This way, everyone on your team will develop in the same environment.

[Homestead](https://laravel.com/docs/8.x/homestead) is another such dev environment.

### Docker based solutions

[WP Docker](https://10up.com/blog/2017/wp-docker/) is another development environment you can use.

The main difference is that Vagrant uses virtual machines to run environments independent of the host machine via [VirtualBox](https://www.virtualbox.org/) or [VMware](http://www.vmware.com/). Each environment has its own virtual machine. Docker, on the other hand, uses 'containers' that include your application and all of its dependencies, but shares the operating system with other containers. They are isolated processes on the host operating system, but are not tied to any specific infrastructure.

**Vagrant** is easier to set up and get up and running, but can be resource-intensive (RAM and space on the disk).
**Docker** is a bit harder to understand and set up, but it is much faster and uses less CPU and RAM than Vagrant.

### Local environment on Unix based systems

You can also set your own dev server on Mac OS or any Unix based system. You'll need to setup your own PHP (FPM), database (MySQL), and a webserver (Apache or Nginx). We usually use Nginx on client projects.

A web service stack based on Linux, Apache, MySQL and PHP is known as a LAMP stack. The Nginx one is known as LEMP stack.

If you want to use a local dev environment you can also check Laravel Valet, or Valet Plus.

### Default local TLD

When setting up local projects it's important to avoid using the reserved `.dev` top level domain (TLD). At Infinum, we chose `.test`, as all our local TLD. VVV and Laravel Valet will set sites up with `.test` by default, in other solutions, you need to make sure to set this up yourself.

If the client project has a production URL 

```
https://clientproject.com
```

your local environment should be 

```
https://clientproject.test
```

In the following chapters, you'll learn how to set up various environments.
