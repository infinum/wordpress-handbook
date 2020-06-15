Although we cannot write fully functional code because PHP is not a functional programming language, we can use some of the functional techniques while writing our code. This can improve code readability and ease of maintenance.

Functions are first-class citizens in PHP:

```php
<?php
// Function as a variable.
$function_name = function() {
  return 42;
};

// Function as a return type.
function create_function() {
  return function() {
    return 'Return from an anonymous function.';
  };
};

$new_func = create_function();

// Function as a parameter.
function display( $function ) {
  echo $func() , "\n\n";
}

// Call a function by name.
call_user_func_array( 'strtoupper', $some_string );

// objects as functions.
class Some_Class {
  public function __invoke( $param1, $param2 ) {
    [...]
  }
}

$instance = new Some_Class();
$instance( 'First', 'Second' ); // call the __invoke() method
```

Even though looping through iterable objects is generally faster if you use `for` or `foreach`, you can use some functional programming techniques that will make your code a bit less _boilerplatey_. Also, the performance effects are [negligible](https://stackoverflow.com/questions/18144782/performance-of-foreach-array-map-with-lambda-and-array-map-with-static-function).

### Mapping

Applying a function to all elements:

```php
<?php
// Old way (imperative):
foreach ( $iterable as $iterable_key => $iterable_value ) {
  $iterable[ $iterable_key ] = strtoupper( $iterable_value );
}

// New way (functional):
$result = array_map( 'strtoupper', $iterable );
```

[array_map manual](https://php.net/manual/en/function.array-map.php)

### Reducing

Reduce an array to a single value:

```php
<?php
// Old way (imperative):
$result = 0;

foreach ( $int_array as $value ) {
  $result += $value;
}

// New way (functional):
$result = array_reduce( $int_array, function( $carry, $item ) { return $carry + $item; }, 0 );
```

[array_reduce manual](https://php.net/manual/en/function.array-reduce.php)

### Filtering

Iterating over an array, but returning only those results that pass some conditions:

[array_filter manual](https://php.net/manual/en/function.array-filter.php)

```php
<?php
// Old way (imperative):
$result = [];

foreach ( $int_array as $value ) {
  if( $value % 2 === 0 ) {
    $result[] = $value;
  }
}

// New way (functional):
$result = array_filter( $int_array, function( $item ) { return ( $item % 2 === 0); } );
```

### Anonymous functions

Using anonymous functions for actions and filters could be problematic because it makes them very hard to unhook later on.

```php
<?php
add_action( 'init', function() {
  call_function();
}, 10 );
```

Do this instead:

```php
<?php
add_action( 'init', 'my_callable_function', 10 );

function my_callable_function() {
  call_function();
}
```

If you are working on a 'public' project that can be extended, you should follow the above instructions. When working on client projects, there is a smaller chance that your hooks will be removed. In that case, anonymous functions can be used as callbacks.

### Closures

In JavaScript, a closure can be thought of as a scope. When you define a function, it silently inherits the scope it is defined in, which is called its closure, and it retains that no matter where it's used. It is possible for multiple functions to share the same closure, and they can have access to multiple closures as long as they are within their accessible scope.

In PHP, a closure is a callable class to which you have bound your parameters manually.

It's a slight distinction, but one that bears mentioning.

A closure is essentially the same as a Lambda, but unlike a Lambda, it can access variables outside the scope that it was created in.

```php
<?php
// Set a multiplier.
 $multiplier = 3;

// Create a list of numbers.
 $numbers = array( 1, 2, 3, 4 );

// Use array_walk to iterate through the list and multiply.
array_walk( $numbers, function( $number ) use ( $multiplier ) {
  echo $number * $multiplier;
} );
```

[Closures in PHP](http://php.net/manual/en/class.closure.php)

### Memoization

Memoization is an optimization technique used to cache function results. If we have a pure function (one that has no side effects), we can cache the result the first time we run it and then just use cache. We can use static variables:

```php
<?php
function factorial( $n ) {
  static $cache = [];

  if ( $n === 1 ) {
    return 1;
  }

  if ( ! array_key_exists( $n, $cache ) ) {
    $cache[ $n ] = $n * factorial( $n - 1 );
  }

  return $cache[ $n ];
}
```

We can generalize this using a helper function:

```php
<?php
function memoize( $func ) {
  return function() use ( $func ) {
    static $cache = [];

    $args = func_get_args();
    $key  = serialize( $args );

    if( ! array_key_exists( $key, $cache ) ) {
      $cache[ $key ] = call_user_func_array( $func, $args );
    }

    return $cache[ $key ];
  }
}
```

and then do:

```php
<?php
$factorial = function( $n ) use( &$factorial ) {
  if( $n === 1 ) {
    return 1
  };

  return $n * $factorial( $n -1 );
}

$mem_factorial = memoize( $factorial );
```

### Links on functional PHP programming

[PHP The Right Way](https://phptherightway.com/pages/Functional-Programming.html)  
[Functional Programming in PHP](https://www.liip.ch/en/blog/functional-programming-in-php)  
[Functional PHP](https://apiumhub.com/tech-blog-barcelona/functional-php/)  
