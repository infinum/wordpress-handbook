When you want to develop a WordPress app, or you are joining an existing project, it is important to have local development ready. There are a number of solutions that can be used.

### Local environment on Unix based systems

You can set your own dev server on macOS or any Unix based system. You'll need to set up your own PHP (FPM), database (MySQL), and a web server (Apache or Nginx). We usually use Nginx as a web server, because that's also what we use on client projects.

A web service stack based on Linux, Apache, MySQL, and PHP is known as a LAMP stack. The Nginx one is known as the LEMP stack.

At Infinum we use [Laravel Herd](https://herd.laravel.com/) as it provides a simple way to set up a local development environment.

### Default local TLD

At Infinum, we use `.test` as our local TLD of choice as it is the default TLD used by Laravel Herd.

If the client project has a production URL:

```
https://infinum.com
```

your local environment should be

```
https://infinum.test
```

In the following chapters, you'll learn how to set up various environments.

Make sure to set up HTTPS for your local development as well. This is important because some browser APIs and features will behave differently on HTTP, and you want to mimic the production environment as closely as possible.
