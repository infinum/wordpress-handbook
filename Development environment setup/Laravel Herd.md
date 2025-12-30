At Infinum we use [Laravel Herd](https://herd.laravel.com/) as it provides a simple way to set up a local development environment. Herd is available for macOS and Windows. Installation on a clean setup is really easy you just [download the installer for MacOS](https://herd.laravel.com/docs/macos/getting-started/installation) or [download the installer for Windows](https://herd.laravel.com/docs/windows/getting-started/installation) and run it. It will install all the necessary software and set up the environment for you.

For more details follow the [official documentation for MacOS](https://herd.laravel.com/docs/macos/sites/managing-sites) or [official documentation for Windows](https://herd.laravel.com/docs/windows/sites/managing-sites)

## Tips and tricks

- Laravel Herd has a Pro version that provides additional features like debugging, profiling, and more. Ask your TL for access to the Herd Pro account.

- If you used any other setup before you should make sure that you don't have any other services running on the same ports as Laravel Herd. If you do, you should stop them before starting Laravel Herd. Also clean up your Mac before starting Laravel Herd.

- Don't worry about **composer**, **node**, **php** packages because Laravel Herd will install everything for you.

## Fixes

As no project is perfect, you might encounter some issues with Laravel Herd. Here are some of the most common ones and how to fix them.

### All 404 images are redirected to home page.

If you are using Laravel Herd and you are getting 404 errors for images the request will redirect to the home page. To fix this and get the correct broken image you can set this script in your laptop and include it to `wp-config.php` file on every project.

**THIS FILE SHOULD NOT BE COMMITTED TO THE REPOSITORY!!!**

File `wp.php` should be placed on your laptop:

```php
<?php

/**
 * This file is used to check if static files exist and return the correct 404 header if they don't.
 * Used with Laravel Herding to ensure the correct 404 header is returned for non-existing files and prevent unnecessary 301 redirects.
 * To use, add the following code to your config.php file and change the path to the wp.php file.
 *
 * require_once '/Users/<your-username>/projects/server/wp.php';
 *
 * THIS FILE SHOULD NOT BE COMMITTED TO THE REPOSITORY!!!
 */

declare(strict_types=1);

if (!isset($_SERVER['REQUEST_URI'])) {
	return;
}

// Find the file path.
$filePath = ABSPATH . \trim(\parse_url($_SERVER['REQUEST_URI'], \PHP_URL_PATH), '/');

// Get the file extension.
$extension = \pathinfo($filePath, \PATHINFO_EXTENSION);

// Check if the file exists and it has an extension so we know it's a static file.
if ($extension && !file_exists($filePath)) {
	// Return 404 header if the file doesn't exist.
	\http_response_code(404);

	// Exit the script.
	exit;
}
```

Usage in `wp-config.php` file:

```php
require_once '/Users/<your-username>/projects/server/wp.php';
```

This will ensure that if the file doesn't exist, it will return a 404 header and prevent the redirect to the home page.
