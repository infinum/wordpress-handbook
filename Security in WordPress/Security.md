When working on enterprise projects, it is vital to follow recommended security practices. Security and risk assessment are a crucial part of software development. Security is also everyone's job and should be embedded in every business process and operation.

## Core concepts

The application security core concepts are primarily concerned with reducing the primary attack surface—our exposure, reachable and exploitable vulnerabilities that we might have in our software. It's crucial to make it as small as possible because the app controls access to the data and sources of software.

The core security process is also known as the 'CIA triad'

* Confidentiality
* Integrity
* Availability

_Confidentiality_ means protecting sensitive data from improper disclosure, which can have legal and contractual consequences for you or the company you work for. One of the things you can do to mitigate this and ensure the confidentiality of your application is to mask and obfuscate access to the application by using passwords, make view-based access controls for accessing the database, and use encryption when dealing with data transfer and storage.

_Integrity_ involves the protection of sensitive data from unauthorized modification and ensuring compliance and security of data processing according to law. One way to ensure data integrity is by using digital signatures ([GPG](https://en.wikipedia.org/wiki/GNU_Privacy_Guard) or [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) using hashes). We need to have proof of the sender's identity—data signed by you so you cannot say you didn't do it/send it, and proof that data was not altered in transit. This is what integrity is all about.

_Availability_ is the prevention of data or system destruction and the ability to maintain operational capability. We need to assess where software could fail (single point of failure), and we need to provide fault tolerance—redundancies that automatically take over the running of the application/service in case of failure. For instance, if the application crashes on one container, another one should automatically boot up to replace it with a minimal timeout. We need to take care of scalability.

## Risk management

Risk can be defined as likelihood (probability) of a threat exploiting a vulnerability, thereby causing damage to an asset.

Risks will regulate the number of controls (limitations) that will be used to reduce the risk to the organization. Risk can be good or bad—investments are risky, but can pay off if you've made a good investment.

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

One secure way would be to use environment variables that can be injected in the app during deployment. Then you can use them in your application with the [`getenv()`](http://php.net/manual/en/function.getenv.php) function.

When working with environment variables in PHP, be sure to disable the `phpinfo()` function, as environment variables are usually visible in it.

Another way, if you are using the AWS and EC2 or ECS instances, could be to store credentials in an encrypted S3 bucket, and pull them using the methods available in [aws-sdk-php](https://aws.amazon.com/sdk-for-php/).

Read more here: https://aws.amazon.com/blogs/security/using-iam-roles-to-distribute-non-aws-credentials-to-your-ec2-instances/

## Security tools

### Code sniffer

The first tool to install in every project is [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer/).

Boilerplate already comes bundled with [coding standards](https://github.com/infinum/coding-standards-wp), which depends on the code sniffer. You can install it using composer:

```bash
composer require infinum/coding-standards-wp --dev
```

The code sniffer tokenizes your code against a set of sniffs that look for certain common practices and possible security issues.

### SonarQube

SonarQube is an open-source quality management platform. It does a static code analysis and checks your code for code smells, bugs, and possible security vulnerabilities.

It can be run from Docker. You can find the installation instructions [here](https://hub.docker.com/_/sonarqube/).

You have to [install Docker](https://docs.docker.com/docker-for-mac/install/) first. After that, pull the SonarQube Docker image

```bash
docker pull sonarqube
```

and start the SonarQube server

```bash
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```

The local SonarQube can be accessed through the browser on `localhost:9000`.

![sonarqube](/img/sonarqube.png)

[Analyzing with scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner) tutorial.

You can log in as admin with the `admin` user and `admin` password. You can add the project by going to `Administration->Projects`. You may be prompted to enter a token name and generate a token. Be sure to remember that token, as it will be used when creating a `sonar-project.properties` file which is necessary to run the scan.

You'll also need to install the Java Virtual Machine (JVM) and `sonar-scanner` using brew

```bash
brew update
brew cask install java

brew install sonar-scanner
```

You'll have to add a `sonar-project.properties` file in the root of the project (`public_html`) that looks something like this:

```
sonar.projectKey=project-key
sonar.projectName=Project Name
sonar.projectVersion=1.0.0
sonar.login=4CqVnlU9PzqKUjBqaHu3oGiPhHAv1PrC4pRrhgOA

sonar.sources=wp-content/themes/,wp-content/plugins/
```

After that, you should go to the project root and run `sonar-scanner`.

In the event of a failure, you can capture the output in a log file with

```bash
sonar-scanner -X &> ~/Desktop/sonar-log.txt
```

After the scan is finished (it will take a while), you'll see the report in the SonarQube UI

![sonarqube report](/img/sonarqube-report.png)

If you already have an existing Docker image, you can start it by

```bash
docker start sonarqube
```

and you can stop it with

```bash
docker stop sonarqube
```
### WPScan

[WPScan](https://wpscan.org/) is a WordPress scanning tool that runs on Docker.

To install WPScan on your Mac, you need to have Docker installed. After you've installed Docker, pull the image with

```bash
docker pull wpscanteam/wpscan
```

Start the scan with

```bash
docker run -it --rm wpscanteam/wpscan --url https://yourblog.com [options]
```

You replace `https://yourblog.com` with your local installation of WordPress. You can see the list of options [here](https://github.com/wpscanteam/wpscan#wpscan-arguments). Or just leave that empty.

That will run WPScan and provide output that looks something like this (output may vary):

```bash
_______________________________________________________________
        __          _______   _____
        \ \        / /  __ \ / ____|
         \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
          \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
           \  /\  /  | |     ____) | (__| (_| | | | |
            \/  \/   |_|    |_____/ \___|\__,_|_| |_|

        WordPress Security Scanner by the WPScan Team
                       Version 2.9.4
          Sponsored by Sucuri - https://sucuri.net
      @_WPScan_, @ethicalhack3r, @erwan_lr, @_FireFart_
_______________________________________________________________

[i] The remote host tried to redirect to: http://my-test-site.test/wp-login.php?redirect_to=http%3A%2F%2Fmy-test-site.test%2F&reauth=1
[?] Do you want follow the redirection ? [Y]es [N]o [A]bort, default: [N] >N
[+] URL: http://my-test-site.test/
[+] Started: Fri Jun  8 13:45:40 2018

[+] Interesting header: SERVER: nginx
[+] XML-RPC Interface available under: http://my-test-site.test/xmlrpc.php   [HTTP 405]
[+] API exposed: http://my-test-site.test/wp-json/   [HTTP 200]
[!] Full Path Disclosure (FPD) in 'http://my-test-site.test/wp-includes/rss-functions.php':

[+] Enumerating WordPress version ...

[+] WordPress version 4.9.6 (Released on 2018-05-17) identified from links opml, advanced fingerprinting

[+] Enumerating plugins from passive detection ...
[+] No plugins found passively

[+] Finished: Fri Jun  8 13:48:27 2018
[+] Elapsed time: 00:02:46
[+] Requests made: 61
[+] Memory used: 35.848 MB
```
## Useful hardening code snippets

### Remove WP version from generator in head
```
  /**
   * Return empty string in the 'wp_generator'
   *
   * @return string Returns an empty string.
   */
  public function empty_generator_version() : string {
    return '';
  }
```
Hooks into (`$security` is a class containing the `empty_generator_version` method):
```
$this->loader->add_filter( 'the_generator', $security, 'empty_generator_version' );
```

### Remove WP version from scripts 
```
  /**
   * Remove the version number from all enqueued scripts
   *
   * @param  string $src Source string of the enqueued file.
   * @return string      Modified string without the version in the enqueued file.
   */
  public function remove_version_scripts_styles( string $src ) : string {
    if ( strpos( $src, 'ver=' ) ) {
      $src = remove_query_arg( 'ver', $src );
    }

    return $src;
  }
```
Hooks into (`$security` is a class containing the `remove_version_scripts_styles` method):
```
$this->loader->add_filter( 'style_loader_src', $security, 'remove_version_scripts_styles', 9999 );
$this->loader->add_filter( 'script_loader_src', $security, 'remove_version_scripts_styles', 9999 );
```
