The Infinum's WordPress boilerplate is the basis of every WordPress based project we use at Infinum.

The repository can be found [here](https://github.com/infinum/wp-boilerplate).

## General info

The Infinum's WordPress boilerplate is an easy way to kickstart your WordPress development using modern programming practices. We wanted to exploit all the advantages of the best PHP practices, which is why we used namespacing to ensure avoiding any possible class, method and function clashes in the global namespace. We also implemented the autoloader which means you no longer need to manually require your classes. Just instantiate them and the autoloader will take care of the rest. With the added performance boost.

We also used Webpack to bundle all the assets (images, JavaScript, fonts and Sass files). This will introduce you to the concepts of modern JavaScript development. Besides all the advantages that Webpack offers (Browsersync plugin that makes cross-device development super easy), it enables you to use all the interesting ES6 features (arrow functions, classes, exports etc.).

There are other added bonuses that you can explore by reading the readme of the project.

This is the in-depth documentation on how to run and modify the WordPress boilerplate code. The intended usage of this boilerplate is to introduce modern front-end development tools to WordPress.

## Getting started

First you need to install WordPress locally, using any of the local development environment you prefer. You can use XAMPP, MAMP, WAMP, VVV, Docker or Laravel Valet. You'll also need to have `Node.js`, `Composer` and `WP-CLI` installed.

To start, fork this repository to your own, and then clone it

```bash
git clone git@github.com:your-name/wp-boilerplate.git
```

If you are using VVV clone it in the `public_html` folder

```bash
git clone git@github.com:your-name/wp-boilerplate.git public_html
```

Run node script to setup your project and rename all files via wizard. Run this first and only once

```bash
npm run rename
```

This will make changes to theme name, description, author, text domain, package, namespace, and constants (this is important when specifying environment variable).

## Development Pre Start

After renaming your theme, run this to setup WordPress on the server.
The script will install `npm` and `composer` dependencies and install the latest version of WordPress.

```bash
bash bin/setup.sh
```

After running setup script, you'll need to create `wp-config.php`. You can do that manually, or use WP-CLI

```bash
wp config create --dbname={DBNAME} --dbuser={DBUSER} --dbpass={DBPASS}
wp core install --url={dev.boilerplate.com} --title={THEMENAME} --admin_user={ADMINUSER} --admin_email={ADMINMAIL}
wp theme activate {THEMENAME}
```

## Development Start

Builds assets in watch mode using Webpack.

```bash
npm start
```

## Browser sync

We are using BrowserSync to sync assets and enable easy cross-device testing.
It is tested on MAMP and Vagrant (VVV).


## Wiki

For more information about the boilerplate check out the official [wiki page](https://github.com/infinum/wp-boilerplate/wiki).
