Below are some useful optimizations that you should be mindful of, when writing your code.

### Array checking

If possible, avoid using the `in_array()` check because it will traverse the entire array and check whether the value of the array is present in the array. Instead, look up by key and use the `isset()` check.

```php
<?php

$array = [
    'foo' => true,
    'bar' => true,
];

if (isset($array['bar'])) {
    // value is present in the array.
};
```

If you want to keep the performance advantage of isset(), while keeping the NULL element correctly detected you can use this combination:

```php
if(isset($array['bar']) || array_key_exists('bar', $array)) {
    // value is present in the array.
}
```

If you have no control over the created array (there are no distinguishable keys to choose from), set the third parameter in the `in_array()` function to `true`. This will force strict comparisons (value and type).

```php
<?php

if (in_array('some value', $array, true)) {
  // code goes here.
}
```

### Using array_push() and similar functions

Avoid using `array_push()` when possible. Instead, just append directly to the array.

```php
<?php
$myArray = [];

// Good.
foreach ($otherArray as $newKey => $newValue) {
  $myArray[] = $newValue;
}

// Avoid if possible.
foreach ( $otherArray as $newKey => $newValue ) {
  array_push($myArray, $newValue);
}
```

This will avoid any unnecessary overhead of calling the PHP function, as PHP has to look up the function reference, find its position in memory, and execute whatever code it defines.

### Caching

Use [caching](https://10up.github.io/Engineering-Best-Practices/php/#the-object-cache) to speed up the site.

Use the [`pre_get_posts`](https://developer.wordpress.org/reference/hooks/pre_get_posts/) hook to modify your queries in the back end and in the front end (search queries, etc.). Be careful to add guard checks so that you affect only the targeted queries.

Use [transients](https://developer.wordpress.org/apis/handbook/transients/) to further speed up your site. If you use caching plugins, they will get stored in the memory instead of the database, so they get called even faster.

### I18n

All text strings in a project have to be internationalized using core localization functions. You can check out this great guide to [internalization in WordPress](https://ottopress.com/2012/internationalization-youre-probably-doing-it-wrong/) by Samuel Wood.

### A11y

Accessibility is important. Always follow the official [WordPress a11y guidelines](https://make.wordpress.org/accessibility/handbook/).
