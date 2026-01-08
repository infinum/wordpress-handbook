When working on enterprise projects, it is vital to follow the recommended security practices. Security and risk assessment are a crucial part of software development. Security is also everyone's job and should be embedded into every business process and operation.

## Core concepts

The application security core concepts are primarily concerned with reducing the primary attack surface—our exposure, reachable and exploitable vulnerabilities that we might have in our software. It is crucial to make it as small as possible because the app controls access to the data and sources of software.

The core security process is also known as the 'CIA triad':

- Confidentiality
- Integrity
- Availability

_Confidentiality_ means protecting sensitive data from improper disclosure, which can have legal and contractual consequences for you or the company you work for. One of the things you can do to mitigate this and ensure the confidentiality of your application is to mask and obfuscate access to the application by using passwords, make view-based access controls for accessing the database, and use encryption when dealing with data transfer and storage.

_Integrity_ involves the protection of sensitive data from unauthorized modification and ensures compliance and security of data processing according to law. One way to ensure data integrity is by using digital signatures ([GPG](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) or [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) using hashes). We need to have proof of the sender's identity—data signed by you so you cannot say you didn't do it/send it, and proof that data was not altered in transit. This is what integrity is all about.

_Availability_ is the prevention of data or system destruction and the ability to maintain operational capability. We need to assess where the software could fail (single point of failure) and provide fault tolerance—redundancies that automatically take over the running of the application/service in case of failure. For instance, if the application crashes on one container, another one should automatically boot up to replace it with a minimal timeout. We need to take care of scalability.

## Risk management

Risk can be defined as the likelihood (probability) of a threat exploiting a vulnerability, thereby causing damage to an asset.

Risks will regulate the number of controls (limitations) that will be used to reduce the risk to the organization. Risk can be good or bad; investments are risky, but can pay off if you've made a good one.

Risk is not eliminated, it is managed. We need to assess the possible risks, respond to them, and monitor them. All this constitutes the risk context or frame.

Once we've assessed the risk, we can choose to: _avoid the risk_ (don't implement a certain feature that is deemed too risky and can present a vulnerability); _transfer the risk_ (third party insurance—for instance, you engage somebody else to do a part of the app); _accept the risk_ (know the limitations and implement the feature); _reduce/mitigate a risk_ (for instance, implement monitoring and logging for an application).

## OWASP

The Open Web Application Security Project (OWASP) is an online community which produces freely available articles, methodologies, documentation, tools, and technologies in the field of web application security.

Check the OWASP page for more information.

[OWASP](https://www.owasp.org/)

[OWASP Top 10](https://www.owasp.org/images/7/72/OWASP_Top_10-2017_%28en%29.pdf.pdf)

[OWASP PHP Top 5](https://www.owasp.org/index.php/PHP_Top_5)

[OWASP Wordpress Security Implementation Guideline](https://www.owasp.org/index.php/OWASP_Wordpress_Security_Implementation_Guideline)

## Some tips

Never store passwords or critical information on Git or any versioning system. Never store `wp-config.php` credentials in your code as well.

One secure way is to use the environment variables that can be injected into the app during deployment. You can then use them in your application with the [`getenv()`](http://php.net/manual/en/function.getenv.php) function.

When working with environment variables in PHP, make sure to disable the `phpinfo()` function, since the environment variables are usually visible in it.

Another way, if you are using the AWS and EC2 or ECS instances, is to store credentials in an encrypted S3 bucket and pull them using the methods available in [aws-sdk-php](https://aws.amazon.com/sdk-for-php/).

Read more here: https://aws.amazon.com/blogs/security/using-iam-roles-to-distribute-non-aws-credentials-to-your-ec2-instances/

## Security tools

### Code sniffer

Boilerplate already comes bundled with [coding standards](https://github.com/infinum/eightshift-coding-standards) which depend on the code sniffer.

### SonarQube

SonarQube is an open-source quality management platform. It does a static code analysis and checks your code for code smells, bugs, and possible security vulnerabilities.

It can be run from Docker. You can find the installation instructions [here](https://hub.docker.com/_/sonarqube/).

### WPScan

[WPScan](https://wpscan.org/) is a WordPress scanning tool that runs on Docker.
