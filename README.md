# WordPress Handbook

This is a handbook of coding standards and practices for WordPress that we use at Infinum. WordPress has evolved a lot since its beginnings — from a useable simple blogging platform to a wannabe CMS moaned by a wide range of projects: simple websites, online shops, complex multi-site projects communicating with a wide range of APIs.

As the usage of WordPress in our company increased, we thought it would be a good idea to define a set of style guides for the projects we work on. We try to follow the WordPress Coding Standards for the most part, but, being a software development company working on a wide range of projects, we already use certain coding standards that differ from default WordPress Coding Standards.

We'll describe our development process here as well—setting the project locally, versioning practices, using linters, code sniffer, etc.

### Development

When fixing typos or updating the handbook, create a new branch from the `master` branch. When you've finished working on it, do a PR towards a `staging` branch. 

`staging` branch will be deployed to the https://beta.infinum.com/handbook/books/wordpress so that we can see if everything is displayed as we wanted. After all is ok, a release branch can be created and merged to the `master` branch, which will be deployed to the production: https://handbook.infinum.co/books/wordpress.

Any code that should be visible to employees only go to the private handbook repository (visible only to people at Infinum).
