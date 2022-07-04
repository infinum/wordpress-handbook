As defined in the [official documentation](https://make.wordpress.org/core/handbook/best-practices/coding-standards/):

>The purpose of the WordPress Coding Standards is to create a baseline for collaboration and review within various aspects of the WordPress open source project and community, from core code to themes to plugins.

>The WordPress community developed the standards contained in this section of the handbook, and those standards are part of the best practices that developers and core contributors are recommended to follow.

At Infinum, we have developed our [own set of coding standards](https://github.com/infinum/coding-standards-wp). You can install them using `Composer`.

```bash
composer require infinum/eightshift-coding-standards
```

If you want to run the checks

```bash
vendor/bin/phpcs --standard=Eightshift .
```

You can customize the commands and output with [additional commands](https://github.com/squizlabs/PHP_CodeSniffer/wiki/Usage).

There are a few key differences between Infinum's coding standards for WordPress and the WordPress Coding Standards. We use PSR-12 standards for our PHP files, and organize our code according to PSR-4 standards. This way we can use Composer's PSR-4 autoloading.
