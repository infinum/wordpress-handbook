# Security

When working on enterprise projects, it is vital to follow the best security practices.

Check the OWASP page for more information.

[OWASP](https://www.owasp.org/)

[OWASP PHP Top 5](https://www.owasp.org/index.php/PHP_Top_5)

[OWASP Wordpress Security Implementation Guideline](https://www.owasp.org/index.php/OWASP_Wordpress_Security_Implementation_Guideline)

## Some tips

Never store passwords, or critical information on the git or any versioning system. Never store `wp-config.php` credentials in the code as well.

One of the secure ways would be to use environment variables that can be injected in the app during deployment. Then you can use them in you application with [`getenv()`](http://php.net/manual/en/function.getenv.php) function.

When working with environment variables in PHP, be sure to disable `phpinfo()` function, as the environment variables are usually visible in it.

Another way, if you are using AWS and EC2 or ECS instances, could be to store credentials in encrypted S3 bucket, and pull them using the methods available in [aws-sdk-php](https://aws.amazon.com/sdk-for-php/).

Read more here: https://aws.amazon.com/blogs/security/using-iam-roles-to-distribute-non-aws-credentials-to-your-ec2-instances/
