Type declarations allow functions to require that parameters are of a certain type at call time. If the given value is of the incorrect type, then an error is generated. In PHP 5, this will be a recoverable fatal error, while PHP 7 will throw a `TypeError` exception. We don't use or encourage using PHP < 8.3 in our projects.

To specify a type declaration, the type name should be added before the parameter name. The declaration can be made to accept `NULL` values if the default value of the parameter is set to `NULL`.

To enable strict typing in PHP, you need to set a `declare` directive at the top of your file, before the `namespace` definition

```php
<?php
declare(strict_types=1);
```

You can type hint function arguments and return values, for example:

```php
<?php
/**
   * Get user data
   *
   * A method that will return an array with user data.
   *
   * @param string $userToken Auth token got from url param.
   *
   * @return array             User data array.
   */
  public function getUserData(string $userToken): array
  {
      //...
  }
```

When the method can return more than one type (for instance, string or boolean), **don't specify the return type**, as this will most likely throw `TypeError` exceptions (you can read the RFC for `mixed` typehint for version 7.3 [here](https://wiki.php.net/rfc/mixed-typehint)).

Since PHP 7.1, you can explicitly declare a variable to be `null`

```php
<?php
public function getArray(?string $someString): array
{
    //...
}
```

Type hinting is also important when working with dependency injections.

```php
<?php
/**
 * Class User credentials
 *
 * Class that stores user credentials.
 */
class UserCredentials {
  /**
   * General Helper object
   *
   * @var string
   *
   * @since 1.0.0
   */
  private $generalHelper;

  /**
   * Initialize class
   *
   * Load helper on class init.
   *
   * @param Helper $generalHelper General helper class that implements Helper interface.
   * @since 1.0.0
   */
  public function __construct(Helper $generalHelper)
  {
      $this->generalHelper = $generalHelper;
  }

  //...
}
```

A table with typehints for each PHP versions: https://mlocati.github.io/articles/php-type-hinting.html
