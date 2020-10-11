> There are only two hard things in Computer Science: cache invalidation and naming things.
> 
> -- <cite>Phil Karlton</cite>


### File naming

File names should be written in lowercase letters with `-` as a separator between words, for example `theme-helpers.php`. In certain cases, such as creating a 'page builder' using ACF, the underscore (`_`) is allowed, but generally you should use the hyphen as a separator. This is used when naming view files.

Files containing a class should be named like the class `{Classname}.php`. There should always be only one class per file. The first letter should be capitalized.

Use [eightshift-boilerplate](https://github.com/infinum/eightshift-boilerplate) as a basis for any template-based website. You should follow the existing structure of the theme there.

### Naming conventions

Use `camelCase` when naming methods and functions. Don't abbreviate variable names unnecessarily — let the code be unambiguous and self-documenting.

### Namespacing and class names

Namespacing should follow the file structure. The main namespace is a CapitalCased version of the project name. So, in the case of boilerplate, the default namespace is

```php
<?php

namespace InfTheme;
```

Every folder that holds classes constitutes a subnamespace.

Be aware that WordPress 'lives' in a global namespace. Therefore, when using functions and classes from the WordPress core, it's necessary to either put a slash in front of the class or function name so that it's called from the global namespace or use the `use` statement at the top of the file. For example, calling `WP_Query` inside your class should be done like this:

```php
<?php

$query = new \WP_Query( $arguments );
```

And core functions should be used like this:

```php
<?php
\wp_unslash( $_POST['someKey'] );
```

or:

```php
<?php
use \WP_Query;

//...

$query = new WP_Query( $arguments );

```

This is especially important if you are writing automated tests.

Avoid using static methods in your classes if possible. Using static methods means that you are calling a function without an instance of the class. But it prevents the usage of many OOP features, such as inheritance, interface implementations, dependency injections, etc. Static methods are useful as helper functions for certain things.

If you are using the same method in many different classes, it is useful to put it in a [Trait](https://php.net/manual/en/language.oop5.traits.php).

A Trait is similar to a class, but only intended to group functionality in a fine-grained and consistent way. It is not possible to instantiate a Trait on its own. It is an addition to traditional inheritance and enables horizontal composition of behavior, that is, the application of class members without requiring inheritance.

Class names should use capitalized words.

```php
<?php
class CustomClass { ... }
```

Constants should be in all uppercase letters with underscores separating the words:

```php
<?php
define( 'IMAGE_URL', get_template_directory_uri() . '/skin/public/images/' );
```

or

```php
<?php
const THEME_VERSION = '1.0.0';
```

### Yoda Conditions

We don't use [Yoda Conditions](https://en.wikipedia.org/wiki/Yoda_conditions), especially since the code checker should make sure that you don't assign a variable in conditionals. They add unnecessary overhead and are hard to read.

### Functions

When defining a function or a method, there should be no space between the function name and the opening parenthesis, and between the closing parenthesis and the open curly bracket.

```php
<?php
function functionName($var) {... }
```

#### Class method visibility

Always add visibility keywords to methods and properties inside classes (`public`, `private`, and `protected`).

* `public` scope is used to make that variable/function available from anywhere; other classes and instances of the object.

* `private` scope is used when you want your variable/function to be visible in its own class only.

* `protected` scope is used when you want to make your variable/function visible in all classes that extend the current class, including the parent class.

If you are working on projects with latest PHP version, use latest features. This means that you should add constant visibiliy modifiers as well if you are working on PHP >= 7.1
