# Versioning for WordPress

*Commit Early, Commit Often*

Your repository should consist of several main branches: `master`, `develop`, `staging`, `edge` and `feature` branches.

There is a difference how a `feature` branch is created pre-release and post-release.

### Pre-release

Before anything is released to production, `staging` or `develop` branches serve as a 'master' branch from which you would branch off.

This way you can be up to date with any development changes (plugins installed etc.).

When submitting a patch, fix or a new feature, always open a new branch, and pull from `develop` branch. For example, a feature would be

`feature/adding-new-user-roles`.

After that create a pull request to `develop` and assign a reviewer.

Every merge is done via pull requests, manual merges are prohibited.

Develop branch is usually connected with a staging server, using build and deploy scripts. After tests are completed, `develop` should be merged with the master branch.

### Post-release

Master branch has to be tagged according to [semver](http://semver.org/) before the release. Short introduction to semver can be found [here](https://www.sitepoint.com/semantic-versioning-why-you-should-using/).

Never merge to `master` or `staging` branch. Project lead is usually the one who will do this periodically.

Every project should have a way to deploy from `staging` branch to a live staging server so that the client can see what is being done on a live server. Likewise, a `master` should be deployed to a live production server.

This should be handled by project lead and devops.

Once a project is in production, every time you create a feature, make a branch off of `master` branch, then once you think it's ready submit a PR to `master` and `develop` branch and tag it accordingly. Once testing is completed, and feature branches are ready, lead developer will deploy them to `master` branch.
