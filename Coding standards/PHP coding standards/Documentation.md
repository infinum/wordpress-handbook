Every file should have introductory documentation that describes the contents of the file. `functions.php` should have a description block about the theme/project.

```php
<?php
/**
 * Theme Name: Project name
 * Description: A short project description
 * Author: Infinum
 * Author URI: https://infinum.co/
 * Version: 0.1.0
 *
 * @package Namespace
 */
```

We follow the [DocBlock](https://phpdoc.org/docs/latest/guides/docblocks.html) format of comments.

Every class should have documentation above it, and the methods inside should also be documented.

```php
<?php
/**
 * Starts the list before the elements are added.
 *
 * @since 3.0.0
 *
 * @see Walker::start_lvl()
 *
 * @param string   $output Passed by reference. Used to append additional content.
 * @param int      $depth  Depth of menu item. Used for padding.
 * @param stdClass $args   An object of wp_nav_menu() arguments.
 */
public function start_lvl( &$output, $depth = 0, $args = array() ) {
  //...
}
```
