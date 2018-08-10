# Versioning for WordPress

*Commit Early, Commit Often*

Repository naming follows this pattern

`wordpress-{project}-{app}`

With allowed characters `[a-z\-]`.

Your repository should consist of several main branches: `master`, `develop`, `staging`, `preproduction`, `edge` and `feature` branches.

### Project setup

Before each project, lead developer should fill the Project Setup sheet which specifies what language and framework will be used, versions, necessary scripts etc.

### Modified Git flow

Before deploy scripts and any kind of CI/CD (Continuous integration/Continuous deployment) is set up by the devops, a `master` branch should be filled with the initial setup. Ideally this should contain renamed theme using the boilerplate and minimum necessary plugins so that the developers can work without always merging the branch with the plugins in it.

In the case that the `master` branch only contains the theme, a feature branch should be made called `feature/add-update-plugins` which holds the necessary plugins, and is used for update and adding the new plugins from the wordpress.org repository, or paid plugins like ACF Pro.

Alywas branch off the `master` branch into feature branch, and never commit directly to it. Usually the master is protected agains that, but better safe than sorry.

**The master branch is always deployable.**

When submitting a patch, fix or a new feature, always open a new branch, and pull from `master` branch. For example, a feature would be

`feature/adding-new-user-roles`.

After that create a pull request to `staging` and `master` (and/or `preproduction`) and assign a reviewer.

Every merge is done via pull requests, manual merges are prohibited.

If the branch you want to work on depends on a branch that is still being worked on, pull the changes from that branch into your branch to get the updates. Be sure to track the changes on that branch.

`staging` branch is usually connected with a staging server, using build and deploy scripts. After tests are completed, features should be merged with the `master` and `preproduction` branches.

![Code flow](/img/code-flow.png)

Master branch has to be tagged according to [semver](http://semver.org/) before the release. Short introduction to semver can be found [here](https://www.sitepoint.com/semantic-versioning-why-you-should-using/).

Never merge to `master` branch. Project lead is usually the one who will do this periodically.

`preproduction` branch should be linked to the preproduction server, so that the client can enter content on it that will be merged to the production once ready. `preproduction` is 1:1 clone of the `master`, and should contain only working and production ready code.

Once a project is in production, every time you create a feature, make a branch off of `master` branch, then once you think it's ready submit a PR to `master`, `preproduction` and `staging` branch and tag it accordingly. Once testing is completed, and feature branches are ready, lead developer will deploy them to `master` branch.

Likewise with the pre-release, if you are working on a feature that depends on another feature that is merged on the staging branch, pull that branch in your branch to get the desired features.

![Git flow](/img/gitflow.png)

**Do not submit PR that has over 100 changed files**

Submitting a huge PR is bad for many reasons. First one is that it can crash your browser if you're inspecting it in Github, second one is that it will take a large amount of time to review it, and most probably the reviewer will skim the PR because he has other tasks to do. When submitting a PR ask yourself: would I want to review this?

Also **write good git commit messages**. Commit messages should be short and clear. Also, write subject in the imperative mood. A properly formed Git commit subject line should always be able to complete the following sentence:

```
If applied, this commit will *your subject line here*
```

More about writing good commit messages can be found [here](https://chris.beams.io/posts/git-commit/).

**Make small commits**, following the Single Responsibility Principle in git. This makes commits easier to review when doing pull requests and it's easier to see what's going on when something goes wrong.

**Don't use git add .** Review what you're adding to your repo - this is the #1 cause of adding unwanted changes.

A good presentation that shows why you should write good commit messages and issues can be watched [here](https://wordpress.tv/2018/07/12/k-adam-white-what-we-forget-to-test/).
