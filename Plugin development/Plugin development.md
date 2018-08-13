# Plugin development

Plugin development is done according to the [WordPress Plugin Boilerplate](http://wppb.io/). You can find a simple generator for your plugin [here](https://wppb.me/).

Note that plugins are written in object oriented way, whereas theme is usually written in procedural way. Object oriented programming allows for better separation of concerns.

We are also working on our version of the plugin boilerplate, and you can ask your team lead about the details for it.

### When should I create a plugin?

Any time you want to create a custom post type or taxonomy, you need to create a plugin.

Any time you are making a remote API call, create a plugin.

When making a modification of the WordPress core APIs it's best to create a plugin for that.

When creating a new REST endpoint create a plugin that will handle it.

Be aware that plugin code executes before the theme ([action reference](https://codex.wordpress.org/Plugin_API/Action_Reference)), to avoid possible issues.

### Won't having many plugins make my site slow?

No, unless they are poorly coded. But our coding standards are high so this tends not to happen.
