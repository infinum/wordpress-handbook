When creating a project on Bitbucket or GitHub, the most important thing is to specify a few crucial details for project setup. This will help with quick onboarding of new developers on the project.

There should be a `README.md` file in the project root folder. At the beginning of this file, you should give a few details about the project and information about the development setup.

A sample readme should look like this:

```md
# Development and deployment

Project _project_ is the project about stuff and some other stuff.

## Development environment

 * **Development URL:** http://dev.projectname.org
 * **Development database:** database_name
 * **Database prefix:** db_

## Staging server
 * The staging server pulls code from the `development` branch

## Pre-production server

  * staging branch
  * pre-production-ready features, test on actual content
  * **DON'T OVERRIDE THE DATABASE**

## Production server
  * master branch
  * only production-ready features
  * **DON'T OVERRIDE THE DATABASE**

## Additional details

For theme quick start check out the [wp-boilerplate](https://github.com/infinum/wp-boilerplate) readme.

Mention some additional details here—for instance, if you have any special deployment techniques from development to staging, additional constants and settings for `wp-config.php`, or similar.
```

Never write data which is meant to be secure in your readme file, or commit them directly to the repository.
