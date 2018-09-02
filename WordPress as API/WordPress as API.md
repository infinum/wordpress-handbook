WordPress app can be used to provide endpoints for the front end app (based on any of the popular view libraries) to consume.

In that case theme should consist of minimum necessary files for admin functionality - `functions.php`, `index.php`, `screenshot.png` (optional) and `style.css`.

Usually `functions.php` contains redirection so that users can never access the 'php' WordPress front

```php
add_action( 'template_redirect', 'inf_theme_redirect' );
if ( ! function_exists( 'inf_theme_redirect' ) ) {
  /**
   * Check if you are on any other page then admin and redirect accordingly.
   *
   * @return void
   */
  function inf_theme_redirect() {
    if ( ! is_user_logged_in() ) {
      auth_redirect();
    } elseif ( ! is_admin() ) {
      wp_redirect( site_url( 'wp-admin' ) );
    }
    exit;
  }
}
```

The endpoints for the front end app should be created either using [REST API](https://developer.wordpress.org/rest-api/), or using [Decoupled JSON Content plugin](https://github.com/infinum/decoupled-json-content).

This functionality should be defined in a plugin.
