### How good is good enough?

In short, your website should load as fast as possible! The ideal load time for mobile sites is 1-2 seconds. A website that takes longer than four seconds to load could easily form a "negative perception" of a company and influence its business as a whole. Such websites most likely won't get their visitors back once they left it.

Every change in content or programming logic is producing changes in overall performance. For example, if a new image is added to the page, it automatically affects total page size, number of requests, and a lot of other variables. It's not enough to just reach the desired performance range and leave it without monitoring. Performance must be perceived as a continuous process.

### Identifying areas of improvement

Waterfall charts are a very valuable tool for detecting problematic areas that need improvement. Analyzing them comes down to recognizing common patterns, and understanding what can be safely ignored.

Because they show loading behavior in a simple way, it's easy to see what was loaded in what order, along with the request details. Also, on a timeline the duration and execution times of each request can be seen, with varying bar-lengths that represent how long that request took to download, and/or execute.

![GTmetrix Waterfall chart](/img/gtmetrix-waterfall-chart.png)

GTmetrix has a very well written [blog post](https://gtmetrix.com/blog/how-to-read-a-waterfall-chart-for-beginners/) which explains Waterfall charts in much more detail.

### Server-side optimization

The performance of the web server should be tested together with the website speed. It's critical to ensure that the web server is properly tuned and its hardware and infrastructure properly sized and optimized. A web server speed test shows metrics on response times, certificate validity, any DNS errors, along with any specific server or DNS error codes, so it can serve as starting point for solving any server-side or DNS issues.

To get a better sense of where all the performance bottlenecks are, it's important to check both server-specific metrics and frontend metrics.

### Practices that could result in bad performance

#### 1. Unoptimized hosting

A low-cost hosting plan is probably using cheaper to run and less optimized servers, which can lead to slow loading times. If the page still loads slowly after reaching a high score on speed tools, upgrading to a better plan or changing hosting service should be taken into consideration.

#### 2. Unoptimized assets (images, scripts and styles)

Uncompressed JS and CSS files and large, unoptimized image files are common issues showing up in test reports. There is a chance that problems like this can never be fully resolved. Assets can always be smaller in size and more optimized. But finding a balance where assets won't lose in quality and still load fast, could be considered as a good compromise.

#### 3. No website cache

All dynamic website technologies talk to the database server each time a user visits the website. Caching resolves this issue by storing web pages for quick retrieval, bypassing the call to the database, and most of the backend programming logic. Not having caching means every request will be freshly loaded from the server which will end up with a slower server response.

### Optimization tips

#### 1. Optimizing and lazy loading images

Images tend to be the heaviest part of web pages. They can have a significant impact on loading speeds. To avoid potential issues, it's best to reduce their file size and dimensions (if possible) before uploading. Also, consider modern image formats like WebP. If you have many images, there are online tools that can batch-optimize images.

Another technique is _lazy loading_. It only loads the image once it's visible to the user. Caution should be taken with this, though, as it can lead to layout shifts and bad user experience if not done properly.

#### 2. Using a CDN

A _Content Delivery Network_ (CDN) is a network of datacenters in multiple locations around the world. It can deliver files to the visitor from the server closest to their physical location. Using one can significantly speed up loading times, as the physical distance the data has to travel will be a lot shorter.

#### 3. Reducing HTTP requests

All the resources a website is using are fetched from the server via HTTP requests, making them a major factor in how fast a web page loads. The fewer HTTP requests, the faster the page will load. One important method and the first step to reducing the number of HTTP requests is to combine multiple scripts into a single script and combine CSS styles into one stylesheet. Don't overdo it, though, sometimes 2 requests are better than 1 if the one request is very large in size!

#### 4. Keeping mobile in mind

As mentioned before, the majority of internet traffic comes from mobile devices. Make sure to properly test and optimize your site for them. Also consider the user experience of your site on mobile, which can impact how long users stay on your site.

#### 5. Implementing caching

Caching allows storage and retrieval of resources from local storage so that a website doesn't have to use resources from its server every time visitors choose to see it. That way files and content are reused rather than loaded fresh. This speeds up page loading time considerably for users accessing the site. There are many different types of caching available (HTTP, page, browser, server...). WordPress, of course, has many plugins which can be used for that type of caching.

And lastly, it's always a good idea to backup data before implementing some major or critical changes.
