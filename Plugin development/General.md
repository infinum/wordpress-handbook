Plugin development is carried out using the [WordPress Plugin Boilerplate](http://wppb.io/). You can find a simple generator for your plugin [here](https://wppb.me/).

Note that plugins are written in an object-oriented way, whereas the theme is usually written in a procedural way. Object-oriented programming allows for better separation of concerns.

We are also working on our version of the plugin boilerplate, which can be found [here](https://github.com/infinum/wp-boilerplate-plugin).

### When should I create a plugin?

When working on enterprise projects, it is easier to bundle the usual plugin content (custom post types, taxonomies, API calls) to the theme instead of a separate plugin. Enterprise projects won't change much over time, so it's useful to have all code in one central place.

If, however, there is some functionality that can be reused in other projects (GDPR plugin or something similar), it is better to put that functionality in a separate plugin.

Be aware that plugin code executes ([action reference](https://codex.wordpress.org/Plugin_API/Action_Reference)) before the theme to avoid possible issues.

### Won't having many plugins slow down my site?

No, unless they are poorly coded. But our coding standards are high, so this usually does not happen.

### Should I write a plugin for everything, or can I use ready-made plugins from the wordpress.org repository?

Write plugins only if you don't find a good free or paid plugin online. For instance, there is no need to write your own contact form plugin when a good plugin is already available—([Contact Form 7](https://wordpress.org/plugins/contact-form-7/)). The same goes for a SEO plugin—([Yoast SEO](https://wordpress.org/plugins/wordpress-seo/)).

Reverse logic also applies—if a plugin for some functionality already exists, but it has a lot of unnecessary overhead code that you don't need, it would be better to write your own plugin (if the time and budget constraints of the project allow it, of course).
