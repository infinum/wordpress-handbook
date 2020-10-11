Below is the list of some code style we agreed upon. These are subjected to change as time goes on.

### Inline statements

Inline statements should have the opening and closing PHP tags on the same line.

```php
<?php
// Yes:
<?php echo esc_html($x); ?>

// No:
<?php
echo esc_html($x);
?>
```

Writing conditionals, or control statements on a single line should be done with braces.

```php
<?php
if ($some_variable === true) { ?>
  <div class="block"></div>
<?php } ?>
```

Never write inline statements without braces. This is an extremely bad practice because it produces code that is hard to read and maintain.

```php
<?php
// Bad code:
if ($some_variable === true)
  $variable = 'This is inside true statement';

// Worse:
if ($foo) bar();

// Good code:
if ($foo) {
  bar();
}
```

### Multiple statements

Each multiple statement should be on its own line

```php
<?php
// Yes:
$x++;
echo esc_html($x);

// No:
$x++; echo esc_html($x); ?>
```

### Strict comparison

Always use strict comparison inside conditionals, `in_array()`, `array_search()`, or any PHP function that has the option to check the value and type of the variable.

It's important to use strict comparison because of [type juggling](https://php.net/manual/en/types.comparisons.php).

If you're not sure what kind of value is returned from the WordPress core function, use [WordPress code reference](https://developer.wordpress.org/reference/) to check.
