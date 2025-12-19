Unit testing is a level of software testing where individual units/components of software are tested. The purpose of unit tests is to validate that each unit of software performs as designed.

In terms of PHP and WordPress, a single 'unit' is a function or a class. Unit testing is dynamic testing of individual units in isolation. This means that the code has to be executed (dynamic). In contrast, static testing is checking for code smells, for which we use linters and code sniffers.

Testing in isolation means that we only execute the code that we want to test and no other unit. We are not interested in coupling. That is what integration tests are for.

An example of a _non-unit_ test would be:

```php
function testRegisterTaxonomy()
{

  $tax = rand_str();

  $this->assertFalse(taxonomy_exists($tax));

  register_taxonomy($tax, 'post');

  $this->assertTrue(taxonomy_exists($tax));
  $this->assertFalse(is_taxonomy_hierarchical($tax));

  unset($GLOBALS['wp_taxonomies'][$tax]);
}
```

You can see that this test calls to multiple WordPress functions like `taxonomy_exists()` and `register_taxonomy()`.

What if your code is dependent on some core functionality? In that case, we need to mock our functions. In the world of unit testing, there are things like mocks, stubs, spies, fake objects, fake functions, dummy functions, test doubles, and other. We won't be going into detail about them. We want to execute real methods for the test without any errors and other fake units without any real logic (mocks) thrown in.

For instance, let's say you have a method that will disable certain REST endpoints in your code. That looks like this:

```php
public function disableDefaultRestFields(array $endpoints): array
{

  // Disable users endpoint.
  if ( isset( $endpoints['/wp/v2/users'] ) ) {
    unset( $endpoints['/wp/v2/users'] );
  }

  if ( isset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] ) ) {
    unset( $endpoints['/wp/v2/users/(?P<id>[\d]+)'] );
  }

  if ( isset( $endpoints['/wp/v2/users/me'] ) ) {
    unset( $endpoints['/wp/v2/users/me'] );
  }

  // Disable media endpoint.
  if ( isset( $endpoints['/wp/v2/media'] ) ) {
    unset( $endpoints['/wp/v2/media'] );
  }

  if ( isset( $endpoints['/wp/v2/media/(?P<id>[\\d]+)'] ) ) {
    unset( $endpoints['/wp/v2/media/(?P<id>[\\d]+)'] );
  }

  return $endpoints;
}
```

You are not interested whether this method is hooked on some action. The only thing you are interested in is that when you pass an array of endpoints to the `disableDefaultRestFields()` method, they don't contain the ones unset in the method. So you'll write a test that looks like this:

```php
class MyTest extends InitTestCase
{
  /**
   * Initial setup for the test
   */
  public function setUp(): void
  {
      parent::setUp();

      // Setup mock endpoints array.
      $this->endpoints = [
          '/wp/v2'                                 => [],
          '/wp/v2/pages'                           => [],
          '/wp/v2/media'                           => [],
          '/wp/v2/media/(?P<id>[\d]+)'             => [],
          '/wp/v2/types'                           => [],
          '/wp/v2/types/(?P<type>[\w-]+)'          => [],
          '/wp/v2/statuses'                        => [],
          '/wp/v2/statuses/(?P<status>[\w-]+)'     => [],
          '/wp/v2/taxonomies'                      => [],
          '/wp/v2/taxonomies/(?P<taxonomy>[\w-]+)' => [],
          '/wp/v2/users'                           => [],
          '/wp/v2/users/(?P<id>[\d]+)'             => [],
          '/wp/v2/users/me'                        => [],
          '/wp/v2/categories'                      => [],
          '/wp/v2/categories/(?P<id>[\d]+)'        => [],
          '/wp/v2/tags'                            => [],
          '/wp/v2/tags/(?P<id>[\d]+)'              => [],
          '/wp/v2/comments'                        => [],
          '/wp/v2/comments/(?P<id>[\d]+)'          => [],
          '/wp/v2/settings'                        => [],
      ];
  }

  /**
   * Tear down after the test ends
   */
  public function tearDown(): void
  {
      parent::tearDown();

      $this->endpoints = null;
  }

  /**
   * Checks disabled rest endpoints
   */
  public function testRestEndpointsAreDisabled()
  {
      $endpoints = $this->endpoints;

      $myClass = new MyClass();

      $filteredEndpoints = $myClass->disableDefaultRestFields($endpoints);

      $this->assertArrayNotHasKey('/wp/v2/users', $filteredEndpoints);
      $this->assertArrayNotHasKey('/wp/v2/users/(?P<id>[\d]+)', $filteredEndpoints);
      $this->assertArrayNotHasKey('/wp/v2/users/me', $filteredEndpoints);
      $this->assertArrayNotHasKey('/wp/v2/media', $filteredEndpoints);
      $this->assertArrayNotHasKey('/wp/v2/media/(?P<id>[\d]+)', $filteredEndpoints);
  }
}
```

We've mocked the endpoint list and used it to check whether the method does what it is supposed to do. We are not interested in whether this will actually remove the endpoints in WordPress. For that, we would have to create integration tests, load WordPress, mock a REST server, and then test if the endpoints are removed.

For more information on unit testing, read [this article](https://tfrommen.de/an-introduction-to-unit-testing-for-wordpress/).

## Brain Monkey

Writing mocks and unit tests from scratch takes a lot of time. That's why we use ready-made packages. One such package is [Brain Monkey](https://giuseppe-mazzapica.gitbook.io/brain-monkey/).

Brain Monkey allows you to mock WordPress functions (just like any PHP function) and check what they are called inside your code.

You can find more details about using Brain Monkey in the [official documentation](https://giuseppe-mazzapica.gitbook.io/brain-monkey/wordpress-specific-tools/wordpress-why-bother).

## WP_Mock

[WP_Mock](https://github.com/10up/wp_mock) is an API mocking framework built and maintained by 10up in order to make proper testing of units within WordPress possible.

You can find the documentation [here](https://github.com/10up/wp_mock/blob/master/README.md).

## Tips and tricks

### Mocking static methods

When your code depends on the outside resources, such as AWS, or other services like Redis, it's best to mock those. Mocking is essentially replacing the real method/class with a fake one that behaves in a similar way.

This is why it's a good idea to wrap your outside dependencies in wrapper classes you can then mock in entirety, without the need to connect to an outside service.

If you need to mock static methods in a class, you need to alias it:

```php
$mock = \Mockery::mock('alias:Namespace\MyClass');
```

Then, you create a mock class and add mocked static methods in it. This alias is used every time a call to a mocked class is found in the code that is being tested.

Bear in mind that using `alias:` will apply for the remainder of the PHP sessions's life, so you'll need to add

```php
/**
 * @runTestsInSeparateProcesses
 * @preserveGlobalState disabled
 */
 ```

to the test class which uses alias mocks. This will tell the PHPUnit to run a separate PHP process so that the other tests wouldn't be affected.

### Overload vs Alias

Taken from [Stack Overflow](https://stackoverflow.com/questions/31219542/what-is-the-difference-between-overload-and-alias-in-mockery):

`Overload` is used to create an "instance mock". This will "intercept" when a new instance of a class is created and the mock will be used instead. For example, if this code is to be tested:

```php
class ClassToTest {
    public function methodToTest()
    {
        $myClass = new MyClass();
        $result = $myClass->someMethod();

        return $result;
    }
}
```

you would create an instance mock using `overload` and define the expectations like this:

```php
public function testMethodToTest()
{
    $mock = Mockery::mock('overload:MyClass');
    $mock->shouldreceive('someMethod')->andReturn('someResult');

    $classToTest = new ClassToTest();
    $result = $classToTest->methodToTest();

    $this->assertEquals('someResult', $result);
}
```

`Alias` is used to mock public static methods. For example, if this code is to be tested:

```php
class ClassToTest
{
    public function methodToTest()
    {
        return MyClass::someStaticMethod();
    }
}
```

you would create an alias mock using `alias` and define the expectations like this:

```php
public function testNewMethodToTest()
{
  $mock = Mockery::mock('alias:MyClass');
  $mock->shouldreceive('someStaticMethod')->andReturn('someResult');

  $classToTest = new ClassToTest();
  $result = $classToTest->methodToTest();

  $this->assertEquals('someResult', $result);
}
```

## Useful links

* [Top 10 PHPUnit Tips and tricks by Juliette Reinders Folmer](https://speakerdeck.com/jrf/my-top-10-phpunit-tips-and-tricks-e6ea54ce-2515-4ea9-aacf-9bf7ab3b3141)
* [Unit Tests for PHP code](https://inpsyde.com/en/php-unit-tests-without-wordpress/)
* [An Introduction To Automated Testing Of WordPress Plugins With PHPUnit](https://www.smashingmagazine.com/2017/12/automated-testing-wordpress-plugins-phpunit/)
* [An Introduction to Unit Testing (for WordPress)](https://tfrommen.de/an-introduction-to-unit-testing-for-wordpress/)
