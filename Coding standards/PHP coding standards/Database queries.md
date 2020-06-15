Querying the database should be done using a `WP_Query` object [Don't use query_posts()](https://wordpress.stackexchange.com/a/1755/58895). It can affect other queries on the page because it reassigns the `global wp_query` object.

You can optimize your query by removing unnecessary queries.

* `no_found_rows => true`: useful when pagination is not needed (AJAX load).
* `update_post_meta_cache => false`: useful when post meta will not be utilized.
* `update_post_term_cache => false`: useful when taxonomy terms will not be utilized.
* `fields => 'ids`': useful when only the post IDs are needed (less typical).

Never use `posts_per_page => -1`, as this will return every single post in the query, which can have detrimental effects if you have a large number of posts. It's better to set a big number (500 or 1000) as the upper limit.

Direct database calls should be discouraged (`$wpdb`), as well as using `get_posts()`. But use them if you cannot avoid them.

Don't use `post__not_in`. If you need to skip posts, do it in PHP

```php
<?php
$custom_query = new WP_Query( array(
    'post_type' => 'post',
    'posts_per_page' => 30 + count( $posts_to_exclude )
) );

if ( $custom_query->have_posts() ) {
  while ( $custom_query->have_posts() ) {
    $custom_query->the_post();
    if ( in_array( get_the_ID(), $posts_to_exclude ) ) {
      continue;
    }
    the_title();
  }
}
?>
```

Try to avoid post meta queries if possible, that is, don't try to fetch posts by their post meta. Instead, use [taxonomies](https://codex.wordpress.org/Taxonomies) to group posts. Taxonomy queries are fast and won't affect your performance.

On the other hand, fetching post meta if you know the post ID or if you are in a post/page is fast and can be used anytime.

```php
<?php
// Don't do this:
$args = array(
    'meta_key'     => 'color',
    'meta_value'   => 'blue',
    'meta_compare' => '!='
);

$query = new WP_Query( $args );

// You can do this:
$color = get_post_meta( get_the_id(), 'color', true );
```

Avoid multi-dimensional queries—post queries based on terms across multiple taxonomies, for instance.

It's better to do a query with the smallest possible number of dimensions, and then filter out the results with PHP.

### Post status security

When setting the `post_status` to anything other than `public`, always add the `'perm' => 'readable'` argument.
This is due to a possible information disclosure vulnerability, which you can read more about here: [WP_Query docs](https://developer.wordpress.org/reference/classes/wp_query/#comment-2378).
