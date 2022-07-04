Git is a versioning software used to work on projects in a collaborative way. It's a developer's bread and butter. It acts as a safety net so that you can undo changes in a code you've written, but also as a documentation of the changes in your code.

We'll cover installing git on your system, and setting it up so that you can start working on the projects in no time.

## Installing git client

### Mac OS

First, try to see if it's already installed on your system

```bash
git --version
```

You may get prompted to install Xcode Command Line Tools. Once you have them installed, you should have git on your system.

In case it wasn't installed, you can install it with Homebrew by running

```bash
HOMEBREW_NO_AUTO_UPDATE=1 brew install git
```


### WSL and Linux

Git should be installed in Ubuntu 20.04. You can verify this by typing

```bash
git --version
```

If it's not installed, you can install it by typing:

```bash
sudo apt update
sudo apt install git
```

## Setting up SSH key pairs

Once git is installed, you'll need to create SSH key pair.

If you don't already have one, create `.ssh` folder inside your home folder

```bash
mkdir -p ~/.ssh && chmod 700 ~/.ssh

```

Then, generate the SSH pair using

```
ssh-keygen -t ed25519 -C "your_email@example.com"
```

**Note**: If you are using a legacy system that doesn't support the Ed25519 algorithm, use:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

Make sure that the `id_rsa` and `id_rsa.pub` keys have been generated inside the `~/.ssh` folder.

Make sure that the correct permissions are set to the private key:

```bash
chmod 600 ~/.ssh/id_rsa
```

You can add the passphrase to the ssh keypair, but git will ask you every time to enter it when you try to push something to the remote repository. In order to avoid that, install keychain

```bash
sudo apt install keychain
```

**Note**: On Mac, keychain should already be installed.

And add the following line to the end of your `~/.bashrc` or `~/.zshrc` (depending on what shell you are using)

```bash
eval "keychain --eval --agents ssh id_rsa"
```

If something is not working as intended follow [this tutorial](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

You can use SSH config file to store some configuration options for the SSH.

By default, the SSH configuration file may not exist, so you may need to create it using the `touch` command :

```bash
touch ~/.ssh/config
```

This file must be readable and writable only by the user and not accessible by others:

```bash
chmod 600 ~/.ssh/config
```

In your config you can add the following

```bash
Host *
  AddKeysToAgent yes
  TCPKeepAlive yes
  ServerAliveInterval 15
  IdentityFile ~/.ssh/id_rsa
```

The `AddKeysToAgent` part will tell SSH to use `ssh-agent` to store keys, the `TCPKeepAlive` and `ServerAliveInterval` will make sure my connection to the server doesn't time out if you walk away from tinkering with it for a while, and the `IdentityFile` just says what the default private key should be used for establishing SSH connections.

## Setup GitHub account

If you don't have a GitHub account, make sure you create it. Once you do it, set up the GitHub credentials inside the `~/.gitconfig` file:

```bash
[user]
  email = john.doe@mail.com
  name = John Doe
  username = johnny_d
[help]
  autocorrect = 0
[diff]
  tool = default-difftool
[difftool "default-difftool"]
  cmd = 'nano'
[core]
  editor = nano
  excludesfile = /Users/john.doe/.gitignore_global
  ignorecase = false
[push]
  default = current
[alias]
  aa = add
  st = status
  co = checkout
  ci = commit
  cl = clone
  ps = push
  br = branch
  pl = pull
  t  = tag
  hist = log --pretty=format:\\"%h %ad | %s%d [%an]\\" --graph --date=short
  type = cat-file -t
  dump = cat-file -p
[color]
  ui = true
[color "diff-highlight"]
  oldNormal = red bold
  oldHighlight = red bold 52
  newNormal = green bold
  newHighlight = green bold 22
[color "diff"]
  meta = yellow
  frag = magenta bold
  commit = yellow bold
  old = red bold
  new = green bold
  whitespace = red reverse
[pull]
  ff = only
```

Set the correct credentials, and feel free to tweak the settings a bit. You can also set up global gitignore file in `~/.gitignore_global`

```bash
# An example global gitignore file
#
# Place a copy of this at ~/.gitignore_global
# Run `git config --global core.excludesfile ~/.gitignore_global`
# Compiled source #
###################

*.com
*.class
*.dll
*.exe
*.o
*.so
*.pyc
*.pyo

# Packages #
############

# it's better to unpack these files and commit the raw source
# git has its own built in compression methods
*.7z
*.dmg
*.gz
*.iso
*.jar
*.rar
*.tar
*.zip
*.msi

# Logs and databases #
######################

*.log
*.sql
*.sqlite

# OS generated files #
######################

.DS_Store
.DS_Store?
._*
.Spotlight-V100
.Trashes
ehthumbs.db
Thumbs.db
desktop.ini

# Temporary files #
###################

*.bak
*.swp
*.swo
*~
*#

# IDE files #
#############

.vscode
.idea
.iml
*.sublime-workspace
*.sublime-project
env
```

This will make sure you don't commit unnecessary files to your remote repository.

Last, but not the least, follow [this tutorial](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) to add a new SSH key to your GitHub account.
