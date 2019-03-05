WordPress has evolved a lot since its beginnings. Apart from being a simple blog CMS, you can use it for other purposes—enterprise solutions for banks and large news portals, eCommerce solutions, web apps, etc.

Here at Infinum, we are always looking to advance our knowledge about modern technologies. This is why we've developed a [wp-boilerplate](https://github.com/infinum/wp-boilerplate). This is sort of a backbone solution to start your project. It uses [Webpack](https://webpack.js.org/) to bundle your files, [Sass](http://sass-lang.com/) for modern and quick styling using [BEM methodology](http://getbem.com/), [ESLint](http://eslint.org/) for JavaScript check, and [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) for checking PHP files.

It also uses OOP principles, such as namespacing, classes, and autoloading using Composer.

Be careful not to pack too many custom functionalities in your theme. All custom functionalities should be placed in a plugin.

## Front end and back end

The back end part (admin interface) is written in PHP. When writing various functionalities, always use WordPress [core functions](https://developer.wordpress.org/) when possible. They are created to work specifically with WordPress core, and will not only make your code more future-proof but safer as well.

Front end can be built with traditional PHP templates, or we can use the [REST API](https://developer.wordpress.org/rest-api/), that has been included in WordPress core since version 4.4, and use modern frameworks like React, Vue, or Angular to build our web apps (decoupled approach).

## Debugging

We've added a `wp-config-project.php` file that has environment-specific constant definitions. Some of those are used for debugging your code.

This is especially useful when you want to watch for any asynchronous calls, like `AJAX` or `fetch()`.

If you want, you can install [Xdebug](https://xdebug.org/), which is a debugger and profiler tool for PHP. It is really useful because you get better looking error messages, profiling and breakpoints in your PHP code.

In addition to that, you can use the [Developer](https://wordpress.org/plugins/developer/) plugin by Automattic when developing a theme or plugin. This is a plugin that installs various useful plugins for development, the most important one being [Query Monitor](https://wordpress.org/plugins/query-monitor/).

### Useful links

[Debugging and profiling php with Xdebug](https://www.sitepoint.com/debugging-and-profiling-php-with-xdebug/)
[View Xdebug cachegrind files on MacOS](http://nickology.com/2014/04/16/view-xdebug-cachegrind-files-on-mac-os/)
[Testing WordPress performance](https://codex.wordpress.org/Testing_WordPress_Performance)
[Code debugging in VVV](https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Code-Debugging)
