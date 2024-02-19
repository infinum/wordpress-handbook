## Theme development

WordPress has evolved a lot since its beginnings. Apart from being a simple blog CMS, you can use it for other purposes — enterprise solutions for banks and large news portals, eCommerce solutions, web apps, etc.

Be careful not to pack too many custom functionalities in your theme. All custom functionalities should be placed in a plugin.

## Plugin development

Plugin development doesn't really differ from the theme development. You can use our boilerplate with libs to build a plugin in pretty much the same way you'd build a theme.

Themes usually handle presentational logic, while plugins handle business logic.

## Front end and back end

The back-end part (admin interface) is written in PHP. When writing various functionalities, use WordPress [core functions](https://developer.wordpress.org/) whenever possible. They are created to work specifically with WordPress core, and will make your code not only more future-proof but also safer.

Front end can be built with traditional PHP templates. We can also use the [REST API](https://developer.wordpress.org/rest-api/) that has been included in the WordPress core since version 4.4, and use modern frameworks such as React, Vue, or Angular to build our web apps (decoupled approach).

## Debugging

We've added a `wp-config-project.php` file that has environment-specific constant definitions. Some of those are used for debugging your code.

This is especially useful when you want to watch for any asynchronous calls, such as `AJAX` or `fetch()`.

If you want to, you can install [Xdebug](https://xdebug.org/) - a debugger and profiler tool for PHP. It is really useful because you get better-looking error messages, profiling and breakpoints in your PHP code.

In addition to that, you can use the [Query Monitor](https://wordpress.org/plugins/query-monitor/) for debugging your database queries, hooks, conditionals, and much more.

## Documentation

Read the [eightshift-docs](https://eightshift.com/) to get started with project development.

### When should I create a plugin?

When working on enterprise projects, it is easier to bundle the usual plugin content (custom post types, taxonomies, API calls) to the theme instead of a separate plugin. Enterprise projects won't change much over time, so it's useful to have all code in one central place.

If, however, there is some functionality that can be reused in other projects (GDPR plugin or something similar), it is better to put that functionality in a separate plugin.

Be aware that plugin code executes ([action reference](https://codex.wordpress.org/Plugin_API/Action_Reference)) before the theme to avoid possible issues.

### Won't be having many plugins slow down my site?

No, unless they are poorly coded. But our coding standards are high, so this usually does not happen.

### Should I write a plugin for everything, or can I use ready-made plugins from the wordpress.org repository?

Write plugins only if you don't find a good free or paid plugin online. For instance, there is no need to write your own contact form plugin when a good plugin is already available—([Contact Form 7](https://wordpress.org/plugins/contact-form-7/)). The same goes for an SEO plugin—([Yoast SEO](https://wordpress.org/plugins/wordpress-seo/)).

Reverse logic also applies — if a plugin for some functionality already exists, but it has a lot of unnecessary overhead code that you don't need, it would be better to write your own plugin (if the time and budget constraints of the project allow it, of course).

## Useful links

[Debugging and profiling PHP with Xdebug](https://www.sitepoint.com/debugging-and-profiling-php-with-xdebug/)  
[View Xdebug cachegrind files on MacOS](http://nickology.com/2014/04/16/view-xdebug-cachegrind-files-on-mac-os/)  
[Testing WordPress performance](https://codex.wordpress.org/Testing_WordPress_Performance)  
[Code debugging in VVV](https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Code-Debugging)  
