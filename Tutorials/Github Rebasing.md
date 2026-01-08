The following is a quick rebasing how to and when tutorial.

**Scenario**: You've made a feature (`feat1`) and you need this feature for some other feature (`feat2`), so that you can continue your work. You can create a new branch from the feature branch and continue developing. During your development time, you've made a PR for the original feature.
The PR has been approved and you merge it to the `staging` branch.

Now you can rebase the new feature to the `staging` branch.

While on the feature branch (`feat2`) commit all that you've developed so far. Check out the `staging` branch

```bash
git checkout staging
```

Pull all the changes to your local `staging` branch.

```bash
git pull origin staging
```

This will depend on your git settings (you might not need to define the `origin` part).

After that, checkout the `feat2` branch

```bash
git checkout feat2
```

Now you can do an interactive rebase

```bash
git rebase -i origin/staging
```

You should see a commit message in the editor that you've set in the `.gitconfig` settings.

You can quit (Cmd-X in Nano, Ctrl-Q in Vim if I'm not mistaken).

Now the interactive rebase process has begun. If you have a conflict it will tell you which file is in the conflicted state. Fix the conflict and write

```bash
git add .
```

This is one of the rare cases you'll want to use the `git add all` command. Usually, you should **NEVER** add everything in one commit unless you have a small change on one feature. By doing that you are losing the granularity and breaking the single responsibility principle (SRP) of your commits. Remember, commits are telling a story. You should be able to read the commit history and understand what the developer was trying to do.

⚠️ **DO NOT COMMIT!!!!!!** (**don't type** `git commit`).

When you've fixed all the conflicts and added the fixed files, type

```bash
git rebase --continue
```

You may need to fix some more conflicts, but maybe you won't.
If all is ok, the rebase will be successful, and you can push your branch with

```bash
git push --force
```

You need to use force push because you've changed the git history and the hashes of the git commits have changed.

You can also use

```bash
git push --force-with-lease origin
```

which is a more secure way to avoid overwriting in case somebody else worked on the remote branch you've just rebased.

That shouldn't be problematic because the good **rule of thumb** is: If two people are working on a single feature branch (which you should never do, each person should work in it's own branch), at the same time, don't rebase. Ever. You're going to change the commit hashes and the other person will get tons of conflicts or errors, and then they'll try to rebase, and you'll chase each other in circles and curse the day you did rebase 😅.

If many people are working on the **same** feature, pull the changes and merge them in your branch, and then tell the person who is working with you to pull the changes.

What are the advantages of rebasing? You have a nice commit history 🙂. Even though you have branched the `feat2` branch from `feat1` branch, after rebasing on the `staging` branch, you'll have all the changes visible as if you've branched from the `staging` branch. The history is nice and linear, and not all over the place.

#### Husky notice

We are using Husky for precommit checks, and if you have lint errors in one of your commits that could bite you in the ass. To avoid Husky running during rebasing run

```bash
HUSKY_SKIP_HOOKS=1 HUSKY=0 git rebase -i ...
```

These two are environment variables that should prevent Husky from running during rebasing.
