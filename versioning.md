# Versioning for WordPress

*Commit Early, Commit Often*

Repository naming follows this pattern

`wordpress-{project}-{app}`

With allowed characters `[a-z\-]`.

Your repository should consist of several main branches: `master`, `develop`, `staging`, `preproduction`, `edge` and `feature` branches.

There is a difference how a `feature` branch is handled pre-release and post-release.

### Project setup

Before each project, lead developer should fill the Project Setup sheet which specifies what language and framework will be used, versions, necessary scripts etc.

### Pre-release

Before anything is released to production, `staging` or `develop` branches serve as a 'master' branch from which you would branch off.

This way you can be up to date with any development changes (plugins installed etc.).

When submitting a patch, fix or a new feature, always open a new branch, and pull from `staging` branch. For example, a feature would be

`feature/adding-new-user-roles`.

After that create a pull request to `staging` and assign a reviewer.

Every merge is done via pull requests, manual merges are prohibited.

If the branch you want to work on depends on a branch that is still being worked on, pull the changes from that branch into your branch to get the updates. Be sure to track the changes on that branch.

Develop branch is usually connected with a staging server, using build and deploy scripts. After tests are completed, `staging` should be merged with the master branch.

![Code flow](/images/code-flow.png)

### Post-release

Master branch has to be tagged according to [semver](http://semver.org/) before the release. Short introduction to semver can be found [here](https://www.sitepoint.com/semantic-versioning-why-you-should-using/).

Never merge to `master` or `staging` branch. Project lead is usually the one who will do this periodically.

Every project should have a way to deploy from `staging` branch to a live staging server so that the client can see what is being done on a live server. Likewise, a `master` should be deployed to a live production server.

This should be handled by project lead and devops.

Once a project is in production, every time you create a feature, make a branch off of `master` branch, then once you think it's ready submit a PR to `master` and `staging` branch and tag it accordingly. Once testing is completed, and feature branches are ready, lead developer will deploy them to `master` branch.

Likewise with the pre-release, if you are working on a feature that depends on another feature that is merged on the staging branch, pull that branch in your branch to get the desired features.

![Git flow](/images/gitflow.png)

