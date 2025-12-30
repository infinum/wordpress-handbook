Querying the database should be done using a `WP_Query` object. [Don't use query_posts()](https://wordpress.stackexchange.com/a/1755/58895). It can affect other queries on the page because it reassigns the `global wp_query` object.

You can optimize your query by removing unnecessary queries.

- `no_found_rows => true`: useful when pagination is not needed (AJAX load).
- `update_post_meta_cache => false`: useful when post meta will not be utilized.
- `update_post_term_cache => false`: useful when taxonomy terms will not be utilized.
- `fields => 'ids`': useful when only the post IDs are needed (less typical).

**Never** use `posts_per_page => -1`, as this will return every single post in the query, which can have detrimental effects if you have a large number of posts. It's better to set a big number (500 or 1000) as the upper limit.

Direct database calls should be discouraged (`$wpdb`), as well as using `get_posts()`. But use them if you cannot avoid them. `$wpdb` is mostly used if you are creating and using custom database tables. `get_posts()` should be avoided, because its results are not cached.

You can read more about uncached calls in the [WordPress VIP documentation](https://docs.wpvip.com/caching/uncached-functions/).

Don't use `post__not_in`. If you need to skip posts, do it in PHP

```php
$postsToExclude = [
    '123' => 1,
    '345' => 1,
    '2' => 1,
    '67' => 1,
];

$customQuery = new WP_Query([
    'post_type' => 'post',
    'posts_per_page' => 30 + count( $postsToExclude )
]);

if ( $customQuery->have_posts() ) {
  while ( $customQuery->have_posts() ) {
    $customQuery->the_post();
    if ( isset($postsToExclude[get_the_id()]) ) {
      continue;
    }

    the_title();
  }
}
```

Try to avoid post meta queries if possible, that is, **don't try to fetch posts by their post meta**. Instead, use [taxonomies](https://wordpress.org/documentation/article/taxonomies/) to group posts. Taxonomy queries are fast and won't affect your performance.

On the other hand, fetching post meta if you know the post ID or if you are in a post/page is fast and can be used anytime.

```php
// Don't do this:
$args = [
    'meta_key' => 'color',
    'meta_value' => 'blue',
    'meta_compare' => '!='
];

$query = new WP_Query($args);

// You can do this:
$color = get_post_meta(get_the_id(), 'color', true);
```

Avoid multi-dimensional queries—post queries. Examples are querying on terms across multiple taxonomies, or querying multiple post meta keys.

```php
// Don't do this:
$args = [
    'post_type' => 'post',
    'tax_query' => [
        'relation' => 'AND',
        [
            'taxonomy' => 'movie_genre',
            'field'    => 'slug',
            'terms'    => ['action', 'comedy'],
        ),
        [
            'taxonomy' => 'actor',
            'field'    => 'term_id',
            'terms'    => [103, 115, 206],
            'operator' => 'NOT IN',
        ],
    ],
];

$query = new WP_Query($args);
```

It's better to do a query with the smallest possible number of dimensions, and then filter out the results with PHP.

### Post status security

When setting the `post_status` to anything other than `public`, always add the `'perm' => 'readable'` argument.

This is due to a possible information disclosure vulnerability, which you can read more about here: [WP_Query docs](https://developer.wordpress.org/reference/classes/wp_query/#comment-2378).
