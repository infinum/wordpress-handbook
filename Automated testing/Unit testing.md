Unit testing is a level of software testing where individual units/components of a software are tested. The purpose of unit tests are to validate that each unit of the software performs as designed.

In terms of PHP and WordPress, a single 'unit' is a function or a class. Unit testing is a dynamic testing of individual units in isolation. That means the code has to be executed (dynamic). In contrast, static tesiting is checking for code smells, for which we use linters and code sniffers.

Testing in isolation means that we only execute the code that we want to test and no other unit. We are not interested in coupling - this is what integration tests are for.

An example of a _non unit_ test would be

```php
function test_register_taxonomy() {

  $tax = rand_str();

  $this->assertFalse( taxonomy_exists( $tax ) );

  register_taxonomy( $tax, 'post' );

  $this->assertTrue( taxonomy_exists( $tax ) );
  $this->assertFalse( is_taxonomy_hierarchical( $tax ) );

  unset( $GLOBALS['wp_taxonomies'][ $tax ] );
}
```

You can see that this test calls to multiple WordPress functions like `taxonomy_exists()` and `register_taxonomy()`.

What if your code depends on some core functionality? In that case we need to mock our functions. In the world of unit testing there are things like mocks, stubs, spies, fake objects, fake functions, dummy functions, test doubles, and other things. We won't be going into detail about them. What we want to do is to execute the real method under test without any errors thrown and that other units are fake ones without any real logic (mocks).

For instance, say you have a method that will disable certain REST endpoints in your code, that looks like this

```php
public function disable_default_rest_fields( array $endpoints ) : array {

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

You are not interested in that this method is hooked on some action. The only thing you are interested in is that when you pass the `disable_default_rest_fields()` method some array of endpoints, that those endpoints won't contain the ones unset in the method. So you'll write a test that looks like this:

```php
class My_Test extends InitTestCase {
  /**
   * Initial set up for the test
   */
  public function setUp() {
    parent::setUp();

    // Setup mock endpoints array.
    $this->endpoints = array(
        '/wp/v2'                                 => array(),
        '/wp/v2/pages'                           => array(),
        '/wp/v2/media'                           => array(),
        '/wp/v2/media/(?P<id>[\d]+)'             => array(),
        '/wp/v2/types'                           => array(),
        '/wp/v2/types/(?P<type>[\w-]+)'          => array(),
        '/wp/v2/statuses'                        => array(),
        '/wp/v2/statuses/(?P<status>[\w-]+)'     => array(),
        '/wp/v2/taxonomies'                      => array(),
        '/wp/v2/taxonomies/(?P<taxonomy>[\w-]+)' => array(),
        '/wp/v2/users'                           => array(),
        '/wp/v2/users/(?P<id>[\d]+)'             => array(),
        '/wp/v2/users/me'                        => array(),
        '/wp/v2/categories'                      => array(),
        '/wp/v2/categories/(?P<id>[\d]+)'        => array(),
        '/wp/v2/tags'                            => array(),
        '/wp/v2/tags/(?P<id>[\d]+)'              => array(),
        '/wp/v2/comments'                        => array(),
        '/wp/v2/comments/(?P<id>[\d]+)'          => array(),
        '/wp/v2/settings'                        => array(),
    );
  }

  /**
   * Tear down after test ends
   */
  public function tearDown() {
    parent::tearDown();

    $this->endpoints = null;
  }

  /**
   * Checks disabled rest endpoints
   */
  public function test_disabled_rest_endpoints() {
    $endpoints = $this->endpoints;

    $my_class = new My_Class();

    $filtered_endpoints = $my_class->disable_default_rest_fields( $endpoints );

    $this->assertArrayNotHasKey( '/wp/v2/users', $filtered_endpoints );
    $this->assertArrayNotHasKey( '/wp/v2/users/(?P<id>[\d]+)', $filtered_endpoints );
    $this->assertArrayNotHasKey( '/wp/v2/users/me', $filtered_endpoints );
    $this->assertArrayNotHasKey( '/wp/v2/media', $filtered_endpoints );
    $this->assertArrayNotHasKey( '/wp/v2/media/(?P<id>[\d]+)', $filtered_endpoints );
  }
}
```

We've mocked the endpoint list, and use it to test if the method does what it needs to do. We are not interested if this will acutally remove the endpoints in the WordPress. For that we'd need to make integration tests, load WordPress and mock a REST server and then test if the endpoints were removed.

For more information on unit testing check [this article](https://tfrommen.de/an-introduction-to-unit-testing-for-wordpress/).

## Brain Monkey

Writing mocks and unit tests from scratch would take a lot of time. That's why we use ready made packages. One such package is [Brain Monkey](https://brain-wp.github.io/BrainMonkey/).

Brain Monkey allows to mock WordPress function (just like any PHP function), and to check how they are called inside your code.

For more details about using Brain Monkey check the [official documentation](https://brain-wp.github.io/BrainMonkey/docs/wordpress-setup.html).

## Useful links

[Unit Tests for PHP code](https://inpsyde.com/en/php-unit-tests-without-wordpress/)
[An Introduction To Automated Testing Of WordPress Plugins With PHPUnit](https://www.smashingmagazine.com/2017/12/automated-testing-wordpress-plugins-phpunit/)
[Unit Tests for WordPress Plugins](https://pippinsplugins.com/unit-tests-wordpress-plugins-introduction/)
[An introduction to unit testing (for WordPress)](https://tfrommen.de/an-introduction-to-unit-testing-for-wordpress/)
