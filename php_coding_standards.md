# PHP coding standards

In general we follow the [WordPress PHP Coding Standards](https://make.wordpress.org/core/handbook/best-practices/coding-standards/php/) with few modifications.

For automatic code check we are using [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer/).

### File Naming

File names should be in lowercase letters with `-` as a separator between words, e.g. `theme-helpers.php`.

Files containing a class should be named `class-{classname}.php`. There should always be only one class per file.

Use [wp-boilerplate](https://github.com/infinum/wp-boilerplate) as a template for any template based web site. You should follow the exisitng structure of the theme there.

### Naming Conventions

Use lowercase letters in variable, action, and function names. Separate words via underscores. Don't abbreviate variable names unnecessarily - let the code be unambiguous and self-documenting.

`function inf_custom_function( $custom_variable ) { ... }`

Class names should use capitalized words separated by underscores.

`class Inf_Custom_Class { ... }`

Constants should be in all upper-case with underscores separating words:

`define( 'IMAGE_URL', get_template_directory_uri() . '/skin/public/images/' );`

Always put a unique prefix to your functions. The prefix we use at Infinum is `inf_`.


### Yoda Conditions

We don't use Yoda Conditions, especially since code checker should take care of making sure that you don't assing a variable in the conditionals.

### Functions

When defining a function there should be no space between a function name and an opening parenthesis, but there should be a space between closing parenthesis and opened curly bracket like so:

`function inf_function_name( $var ) { ... }`

Don't use anonymous functions for actions and filters because they make it very hard to unhook later on

```
add_action( 'init', function() {
  call_function();
}, 10 );
```

Instead do this

```
add_action( 'init', 'my_callable_function', 10 );

function my_callable_function() {
  call_function();
}
```

### Sanitization and escaping

We follow WordPress [VIP's guidelines](https://vip.wordpress.com/documentation/vip/best-practices/security/validating-sanitizing-escaping/)

1. Never trust user input.
2. Escape as late as possible.
3. Escape everything from untrusted sources (like databases and users), third-parties (like Twitter), etc.
4. Never assume anything.
5. Never trust user input.
6. Sanitation is okay, but validation/rejection is better.
7. Never trust user input.

Every output has to be escaped. Even translatable strings. This means that instead of using `__()` and  `_e()` we need to use `esc_html__()`, `esc_html_e()`, `esc_attr__()`, `esc_attr_e()`, `wp_kses()`, `wp_kses_post()` and other escaping functions.

When writing data to the database be sure to sanitize the variables

`sanitize_text_field( wp_unslash( $_POST['my_data'] ) )`

And to [prepare](https://developer.wordpress.org/reference/classes/wpdb/prepare/) your database queries.


