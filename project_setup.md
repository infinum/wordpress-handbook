# Project setup

When creating a project on bitbucket or github, it is paramount to specify few crucial details for project setup. This will help with quick on boarding of new developers on the project.

In the project root folder there should be a `README.md` file. At the beginning of this file you should give few details about the project and details about the development setup.

A sample readme should look like this:

```md
# Project readme

Project _project_ is the project about stuff and some other stuff.

## Development details

*Development URL:* http://dev.projectname.org
*Development database:* database_name
*Database prefix:* db_

## Additional details

For theme quick start check [wp-boilerplate](https://github.com/infinum/wp-boilerplate) readme.

Mention some additional details here. For instance if you have any special deployment techniques from development to staging.
```

You can include user name and password for the development database, but this is not that important, since it can vary depending on the development environment.
