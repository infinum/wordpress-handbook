When you want to develop a WordPress app, or you are joining an existing project, it is important to have local development ready. There are a number of solutions that can be used.

### Local environment on Unix based systems

You can set your own dev server on macOS or any Unix based system. You'll need to set up your own PHP (FPM), database (MySQL), and a web server (Apache or Nginx). We usually use Nginx as a web server, because that's also what we use on client projects.

A web service stack based on Linux, Apache, MySQL, and PHP is known as a LAMP stack. The Nginx one is known as the LEMP stack.

At Infinum we use [Laravel Herd](https://herd.laravel.com/) as it provides a simple way to set up a local development environment.

### Default local TLD

When setting up local projects it's important to **avoid using the reserved `.dev` top-level domain (TLD)**. At Infinum, we chose `.test`, as all our local TLD because Laravel Herd works using this TLD.

If the client project has a production URL:

```
https://clientproject.com
```

your local environment should be 

```
https://clientproject.test
```

In the following chapters, you'll learn how to set up various environments.
