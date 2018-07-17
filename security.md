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

## Security tools

### Code sniffer

The first tool to install in every project is [PHP_CodeSniffer](https://github.com/squizlabs/PHP_CodeSniffer/).

Boilerplate already comes bundled with [coding standards](https://github.com/infinum/coding-standards-wp), which depends on code sniffer. Using composer you can install it like this

```bash
composer require infinum/coding-standards-wp --dev
```

Code sniffer tokenizes your code against a set of sniffs, that watch for best practices and possible security issues.

### SonarQube

SonarQube is an open source quality managment platform. It will make a static code analysis and check your code for code smells, bugs and possible security vulnerabilities.

It can be run from docker. The installation instructions can be found [here](https://hub.docker.com/_/sonarqube/).

You need to [install docker](https://docs.docker.com/docker-for-mac/install/) first. After that pull the sonarqube docker image

```bash
docker pull sonarqube
```

And start the sonarqube server

```bash
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
```

The local SonarQube can be accessed in the browser on `localhost:9000`.

![sonarqube](/images/sonarqube.png)

[Analyzing with scanner](https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner) tutorial.

You can log in as admin with `admin` user and `admin` password. You can add the project by going to `Administration->Projects`. You may be prompted to enter a token name and generate a token. Be sure to remember that token, as it will be used when creating a `sonar-project.properties` file needed to run the scan.

You'll also need to install Java Virtual Machine (JVM) and `sonar-scanner` using brew

```bash
brew update
brew cask install java

brew install sonar-scanner
```

In the project root you'll need to add a `sonar-project.properties` file in the root of the project (`public_html`) that looks something like this

```
sonar.projectKey=project-key
sonar.projectName=Project Name
sonar.projectVersion=1.0.0
sonar.login=4CqVnlU9PzqKUjBqaHu3oGiPhHAv1PrC4pRrhgOA

sonar.sources=sonar.sources=wp-content/themes/,wp-content/plugins/
```

After that you should go to the project root and run `sonar-scanner`.

In the event of a failure, you can capture the output in a log file with

```bash
sonar-scanner -X &> ~/Desktop/sonar-log.txt
```

After a scan is done (should take a while) you'll see the report in the SonarQube UI

![sonarqube report](/images/sonarqube-report.png)

If you already have an existing docker image, you can start it by

```bash
docker run sonarqube
```

### WPScan

[WPScan](https://wpscan.org/) is a WordPress scanning tool that runs on docker.

To install WPScan on your mac, you need to have docker installed. After you installed docker pull the image with

```bash
docker pull wpscanteam/wpscan
```

Start the scan with

```bash
docker run -it --rm wpscanteam/wpscan -u https://yourblog.com [options]
```

You would change `https://yourblog.com` with your local installation of WordPress. You can see the option list [here](https://github.com/wpscanteam/wpscan#wpscan-arguments). Or just leave that empty.

That will run the WPScan and provide output that looks something like this (output may vary)

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


