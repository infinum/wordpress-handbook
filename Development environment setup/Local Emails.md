To avoid spamming or sending emails from your local enviroment to the real world users you should always setup your local enviroment to use a local mail server. We recommend using [Mailpit](https://mailpit.axllent.org/) or [Mailhog](https://github.com/mailhog/MailHog).

Installation for any of these services is really easy and you can find the instructions on their official websites.

After the installation you should configure your local enviroment to use the local mail server by updating your __php.ini__ file with the following settings:

```ini
sendmail_path = '/opt/homebrew/bin/mailpit sendmail'
```
