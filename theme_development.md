# Theme development

WordPress has evolved a lot since its beginnings. Apart from being a simple blog CMS, you can use it for other purposes - enterprise solutions for banks and large news portals, eCommerce solution, web apps and other.

At Infinum we are always looking to advance our knowledge about modern technologies. This is why we developed a [wp-boilerplate](https://github.com/infinum/wp-boilerplate). This is sort of a back bone solution to start your project. It uses [webpack](https://webpack.js.org/) to bundle your files, [Sass](http://sass-lang.com/) with [susy](http://susy.oddbird.net/) for modern and quick styling using [BEM metodology](http://getbem.com/), [ESLint](http://eslint.org/) for javascript check and [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer) for php check.

Be careful not to pack too many custom functionalities in your theme. For any custom post types, create a plugin.

## Front end and back end

Back end part (admin interface) is written in php. When writing various functionality, always use WordPress [core functions](https://developer.wordpress.org/) when possible. They are created to specifically work with WordPress core, and will not only make your code more future proof, but safer as well.

Front end can be built with traditional php templates, or we can use the [REST API](https://developer.wordpress.org/rest-api/) that has been included in WordPress core since version 4.4, to use modern frameworks like React, Vue or Angular to build our web apps.