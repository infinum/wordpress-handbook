Every file should have introductory documentation that describes the contents of the file. `functions.php` should have a description block about the theme/project.

```php
/**
 * Theme Name: Project name
 * Description: A short project description
 * Author: Infinum
 * Author URI: https://infinum.com/
 * Version: 0.1.0
 *
 * @package Namespace
 */
```

We follow the [DocBlock](https://docs.phpdoc.org/guide/guides/docblocks.html#more-on-docblocks) format of comments.

Every class should have documentation above it, and the methods inside should also be documented.

```php
/**
 * Starts the list before the elements are added.
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

When documneting your code, take your time to make a sensible documentation. Most of the time, methods can be written in a way that they are self-documenting.

In some cases, when you're dealing with a complex business logic, you'd write the functionality in more details. That way, when you're handing the project off, or need to revisit it in six months, you'll know what that code does.
