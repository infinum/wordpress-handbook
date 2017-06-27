# Versioning for WordPress

*Commit Early, Commit Often*

Your repository should consists of several main branches: `master`, `development`, `staging` and subbranches.

The latest development branch is always the `development`. When submitting a patch, fix or a new feature, always open a new branch, and pull from `development` branch. For example, a bugfix branch would look like

`bugfix/task-123-missing-post-image`

A feature would be

`feature/adding-new-user-roles`.

After that create a pull request to development and assign a reviewer.

Development is periodically merged with `staging` branch that is used for user testing. After tests are completed, `development` should be merged with the master branch.

Master branch has to be tagged according to [semver](http://semver.org/) before the release. Short introduction to semver can be found [here](https://www.sitepoint.com/semantic-versioning-why-you-should-using/).

Never merge to `master` or `staging` branch. Project lead is usually the one who will do this periodically.

Every project should have a way to deploy from `staging` branch to a live staging server so that the client can see what is being done on a live server. Likewise, a `master` should be deployed to a live production server.

This should be handled by project lead and devops.