*Commit early, commit often*

Naming a repository follows this pattern:

`wordpress-{project}-{app}`

with allowed characters `[a-z\-]`.

Your repository should consist of several main branches: `master`, `develop`, `staging`, `preproduction`, `edge`, and `feature` branch.

### Project setup

Before each project starts, the lead developer should fill out the Project Setup Sheet that specifies what language and framework will be used, versions, necessary scripts, deployment setup, server requirements, etc.

### Modified Git flow

Before the DevOps set up deploy scripts and any kind of CI/CD (Continuous Integration/Continuous Deployment), a `master` branch should be filled with the initial setup. Ideally, this should contain a renamed theme using the boilerplate and, possibly, the minimum necessary plugins so that the developers can work without always having to merge the branch with the plugins in it.

In the case that the `master` branch contains only the theme, a feature branch called `feature/add-update-plugins` which holds the necessary plugins should be made. It is used for updates and for adding new plugins from the wordpress.org repository or paid plugins, such as ACF Pro.

Always branch off the `master` branch into a feature branch, and never commit directly to it. The master is usually protected against that, but better safe than sorry.

**The master branch is always deployable**

When submitting a patch, fix or a new feature, always open a new branch and pull from the `master` branch. For example, a feature would be:

`feature/adding-new-user-roles`.

After that, create a pull request to `staging` and `master` (and/or `preproduction`), and assign a reviewer.

Every merge is done via pull requests. Manual merges are prohibited (or not possible in the case of a `master` branch).

If the branch you want to work on depends on a branch that is still being worked on, pull the changes from that branch into your branch to get updates. Be sure to track the changes on that branch.

The `staging` branch is usually connected with a staging server using build and deploy scripts. After the tests are completed, the features should be merged with the `master` and `preproduction` branches.

![Code flow](/img/code-flow.png)

The `master` branch has to be tagged according to [SemVer](http://semver.org/) before the release. A short introduction to SemVer can be found [here](https://www.sitepoint.com/semantic-versioning-why-you-should-using/).

The `master` branch is usually protected so that it is not possible to merge directly to it. Merges to `master` are done periodically when new features are ready to be released.

The `preproduction` branch should be linked to the preproduction server so that the client can enter content that will be merged to the production once ready. The `preproduction` is a 1:1 clone of the `master` and should contain only working and production-ready code.

Once the project is in production, you should make a branch from the `master` branch every time you create a feature. When you think it's ready, submit a PR to the `master`, `preproduction`, and `staging` branch, and tag (label) it accordingly. Once the testing is completed and feature branches are ready, the lead developer will deploy them to the `master` branch.

It's the same with the pre-release. If you are working on a feature that depends on another feature merged to the staging branch, pull that branch to your branch to get the desired features.

![Git flow](/img/gitflow.png)

**In case of conflicts, don't panic**

Once you have fixed the possible issues in the PR, make sure the conflicts are fixed.

**Don't fix conflicts on GitHub**. Use terminal. Say you have two PRs open—one to the `staging` and one to the `master` branch. In case of conflicts on the PR toward the staging branch, you will want to do the following:

```bash
git checkout staging
git merge --no-ff feature/conflict-branch-name
git push staging
```

Once you have merged the feature branch to staging, you'll have conflicts. Fix them, commit them, and push to staging. This won't impact the other PR to the master. If you use the first GitHub suggestion or their edit tool, you will merge the entire staging branch to your feature branch, and that will impact the PR to the master branch (a bunch of unwanted commits and messed up history).

If you're not sure what to do, ask someone who is. :)

**Do not submit PR that has over 100 changed files**

There are many reasons why submitting a huge PR is bad. The first one is that it can crash your browser if you are inspecting it on GitHub. The second one is that it will take a lot of time to review it, and the reviewer will most probably just skim through the PR because they have other things to do. When submitting a PR, ask yourself, "Would I want to review this?"

Also **write good Git commit messages**. Commit messages should be short and clear. Write the subject in the imperative mood. A properly formed Git commit subject line should always complete the following sentence:

```
If applied, this commit will *your subject line here*
```

More on writing good commit messages can be found [here](https://chris.beams.io/posts/git-commit/).

**Make small commits**, following the Single Responsibility Principle on Git. This makes commits easier to review when doing pull requests, and it's easier to see what is going on when something goes wrong.

**Don't use git add .** Review what you're adding to your repo—this is the #1 cause of making unwanted changes.

[Here](https://wordpress.tv/2018/07/12/k-adam-white-what-we-forget-to-test/) you can watch a presentation on common issues and reasons for writing good commit messages.
