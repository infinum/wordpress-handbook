# Theme development

WordPress has evolved a lot since its beginnings. Apart from being a simple blog CMS, you can use it for other purposes - enterprise solutions for banks and large news portals, eCommerce solution, web apps and other.

At Infinum we are always looking to advance our knowledge about modern technologies. This is why we developed a [wp-boilerplate](https://github.com/infinum/wp-boilerplate). This is sort of a back bone solution to start your project. It uses [Webpack](https://webpack.js.org/) to bundle your files, [Sass](http://sass-lang.com/) for modern and quick styling using [BEM methodology](http://getbem.com/), [ESLint](http://eslint.org/) for JavaScript check and [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) for php check.

It also uses OOP principles such as namespacing, classes and autoloading using Composer.

Be careful not to pack too many custom functionalities in your theme. For any custom post types, create a plugin.

## Front end and back end

Back end part (admin interface) is written in php. When writing various functionality, always use WordPress [core functions](https://developer.wordpress.org/) when possible. They are created to specifically work with WordPress core, and will not only make your code more future proof, but safer as well.

Front end can be built with traditional php templates, or we can use the [REST API](https://developer.wordpress.org/rest-api/) that has been included in WordPress core since version 4.4, to use modern frameworks like React, Vue or Angular to build our web apps.

## Debugging

We've added a `wp-config-project.php` file that has environment specific constant definitions. Some of those are used for debugging your code.

This is especially useful when you want to watch for any asynchronous calls, like `AJAX` or `fetch()`.

Optionally you can install [Xdebug](https://xdebug.org/), which is a debugger and profiler tool for PHP. It is really useful because you get better looking error messages, profiling and breakpoints in your php code.

Besides that, when developing a theme or a plugin, you can use [Developer](https://wordpress.org/plugins/developer/) plugin by Automattic. This is a plugin that installs various useful plugins for development. The most important one being [Query Monitor](https://wordpress.org/plugins/query-monitor/).
