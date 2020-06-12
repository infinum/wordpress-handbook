Below are some useful optimizations that you should be mindful of, when writing your code.

### Array checking

If possible, avoid using the `in_array()` check because it will traverse the entire array and check whether the value of the array is present in the array. Instead, look up by key and use the `isset()` check.

```php
<?php
$array = array(
  'foo' => true,
  'bar' => true,
);

if ( isset( $array['bar'] ) ) {
  // value is present in the array.
};
```

If you have no control over the created array (there are no distinguishable keys to choose from), set the third parameter in the `in_array()` function to `true`. This will force strict comparisons (value and type).

```php
<?php
if ( in_array( 'some value', $array, true ) ) {
  // code goes here.
}
```

### Using array_push() and similar functions

Avoid using `array_push()` when possible. Instead, just append directly to the array.

```php
<?php
$my_array = array();

// Good.
foreach ( $other_array as $new_key => $new_value ) {
  $my_array[] = $new_value;
}

// Avoid if possible.
foreach ( $other_array as $new_key => $new_value ) {
  array_push( $my_array, $new_value );
}
```

This will avoid any unnecessary overhead of calling the PHP function, as PHP has to look up the function reference, find its position in memory, and execute whatever code it defines.

### Caching

Use [caching](https://10up.github.io/Engineering-Best-Practices/php/#the-object-cache) to speed up the site.

Use the [`pre_get_posts`](https://developer.wordpress.org/reference/hooks/pre_get_posts/) hook to modify your queries in the back end and in the front end (search queries, etc.).

Use [transients](https://codex.wordpress.org/Transients_API) to further speed up your site.

### I18n

All text strings in a project have to be internationalized using core localization functions. You can check out this great guide to [internalization in WordPress](https://ottopress.com/2012/internationalization-youre-probably-doing-it-wrong/) by Samuel Wood.

### A11y

Accessibility is important. Always follow the official [WordPress a11y guidelines](https://make.wordpress.org/accessibility/handbook/).
