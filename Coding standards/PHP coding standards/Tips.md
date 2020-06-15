Below are some useful tips and tricks to keep in mind when developing in WordPress.

## Looping custom posts on the post screen

When looping posts on the admin screen, usually in the plugin, you'd be tempted to use normal 'The Loop':

```php
if ( $custom_query->have_posts() ) {
  while ( $custom_query->have_posts() ) {
    $custom_query->the_post();
    // Stuff happens here
  }
  wp_reset_postdata();
}
```

There seems to be a bug in the core [#18408](https://core.trac.wordpress.org/ticket/18408). This bug affects the post data if you create a custom `WP_Query` object before the editor has been outputted. The post from this custom query will fill the edit post form.
This happens because `$wp_query->post` is never defined in the admin load, which `wp_reset_postdata()` relies on to reset the original post data.

To remedy this you can loop through your posts by not setting the `$custom_query->the_post()`.

```php
if ( $custom_query->have_posts() ) {
  foreach ( $custom_query->get_posts() as $post ) {
    $post_id = $post->ID;
  }
}
```

This will drop the need to reset post data, and the regular post will remain in the content.
