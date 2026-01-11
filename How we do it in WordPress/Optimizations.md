In the last few years, the internet has become a place where consumers make their buying decisions and purchase products. It is taking over the role of "friend's recommendation" in many industries. You can turn people who had previously never heard of your company into customers just by getting recognized.

Many companies have realized the value that a website brings to their business - now it's one of their most important resources! Today, if you want to get the most out of your website, it needs to be fast. Most visitors take only a few seconds to decide whether they like what you're presenting or if they should go someplace else.

In simple words, **website optimization** can be defined as the process of using tools, strategies, and experiments to improve the performance of a website, drive more traffic, increase conversions, and grow revenue. It's a lengthy process, but it provides many benefits. Some of them can be seen right away, some take time to manifest.

## Mandatory optimizations

There are a few mandatory optimizations that you should always follow.

### 1. Optimizing the code to load only scripts used

You should always optimize the code to load only scripts used. This will reduce the number of requests and the size of the files. We use DynamicImport for dynamic imports of scripts and assets.

For more information about DynamicImport, check the [DynamicImport](https://javascript.info/modules-dynamic-imports).

### 2. Optimizing the assets to load only images used, like lazy loading, webP, etc.

You should always optimize the assets to load only images used. This will reduce the number of requests and the size of the files. We use LazyLoad for lazy loading of images. When you are using UI-Kit and Utils plugins all images are already lazy loaded and optimized with webP conversion on upload.

### 3. Optimizing the WP_Query.

You should always optimize the WP_Query to load only the posts needed within the context of the page. This will reduce the number of requests and speed up the page load time.

For more information about WP_Query, check the [WP_Query documentation](/how-we-do-it-in-wordpress/coding-standards/php-coding-standards/optimizations).

### 4. Optimizing the database queries.

When writing custom database queries, you should always optimize the queries to load only the data needed and implement caching for the queries.

For more information about database queries, check the [Database queries documentation](https://developer.wordpress.org/reference/classes/wp_object_cache/).

### 5. Use Query Monitor to optimize the queries and profiling.

Always use Query Monitor to check what is happening with the queries and profiling and also to troubleshoot any issues with the queries.

### 6. Setup caching for the project.

For non-AWS projects we use WP Rocket plugin for caching, and for AWS projects we use CloudFront in combination with Eightshift Utils plugin for caching.

### 7. Run Lighthouse and PageSpeed Insights tests.

You should always run Lighthouse and PageSpeed Insights tests to check the performance of the website.

For more information check the [Testing and analyzing](/how-we-do-it-in-wordpress/optimizations/testing-and-analyzing) documentation.
