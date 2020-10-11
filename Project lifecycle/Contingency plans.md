When deploying new versions on the production things can go awry.

This is a scenario report in case the production goes down, what will happen and what steps need to happen to get it back up as soon as possible.

* Notify DevOps engineers to start working on finding root cause (check resource usage, bottlenecks, etc)
* Check logs - error logs, access logs for suspicious activity
* Disable plugins one by one and check if the issue persists
* Change the theme to the default one (if possible using WP-CLI)
* Flush cache and clear transients
* Reset permalink settings
* Check logs - error logs (max 5 min)
* Add maintenance page notice
* Revert the latest stable release
* The whole process shouldn't take more than 20 minutes in total if possible, to minimize the impact to the end users.
