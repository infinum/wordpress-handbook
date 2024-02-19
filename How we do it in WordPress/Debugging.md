## Debugging

We've added a `wp-config-project.php` file that has environment-specific constant definitions. Some of those are used for debugging your code.

This is especially useful when you want to watch for any asynchronous calls, such as `AJAX` or `fetch()`.

If you want to, you can install [Xdebug](https://xdebug.org/) - a debugger and profiler tool for PHP. It is really useful because you get better-looking error messages, profiling and breakpoints in your PHP code.

In addition to that, you can use the [Query Monitor](https://wordpress.org/plugins/query-monitor/) for debugging your database queries, hooks, conditionals, and much more.
