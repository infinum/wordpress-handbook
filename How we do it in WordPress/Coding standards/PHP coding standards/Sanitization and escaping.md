We follow WordPress [VIP's guidelines](https://docs.wpvip.com/security/validating-sanitizing-and-escaping/)

1. Never trust user input.
2. Escape as late as possible.
3. Escape everything from untrusted sources (like databases and users), third-parties (like Twitter), etc.
4. Never assume anything.
5. Never trust user input.
6. Sanitation is okay, but validation/rejection is better.
7. Never trust user input.

Every output **has to be escaped**. Even translatable strings. This means that instead of using `__()` and `_e()`, we have to use `esc_html__()`, `esc_html_e()`, `esc_attr__()`, `esc_attr_e()`, `wp_kses()`, `wp_kses_post()`, and other escaping functions.

When writing data to the database, be sure to [sanitize](https://docs.wpvip.com/security/validating-sanitizing-and-escaping/#h-sanitizing-cleaning-user-input) the variables

`sanitize_text_field( wp_unslash( $_POST['my_data'] ) )`

and to [prepare](https://developer.wordpress.org/reference/classes/wpdb/prepare/) your database queries.

```php
$meta = 'Custom meta';

$postId = $_GET['post_id'];

$wpdb->query(
    $wpdb->prepare(
        "DELETE FROM $wpdb->postmeta WHERE post_id = %d AND meta_key = %s",
        $postId, $meta
    )
);
```

This is especially important when dealing with user input from front end (forms).
