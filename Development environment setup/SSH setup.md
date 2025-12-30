Git is a versioning tool, used to keep track of the changes in your code. It’s also used to collaborate with other developers on the code you write. It is a form of distributed version control where the complete codebase, including its full history, is mirrored on every developer's computer.

Compared to centralized version control, this enables automatic management branching and merging, improves the ability to work offline, and does not rely on a single location for backups.

It's a developer's bread and butter. It acts as a safety net so that you can undo changes in a code you've written, but also as a documentation of the changes in your code.

We'll cover installing git on your system, and setting it up so that you can start working on the projects in no time.

## Installing git client

### macOS

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

Make sure that the `id_rsa` and `id_rsa.pub` keys (or `id_ed25519` and `id_ed25519.pub` keys). have been generated inside the `~/.ssh` folder.

Make sure that the correct permissions are set to the private key:

```bash
chmod 600 ~/.ssh/id_rsa
```

or

```bash
chmod 600 ~/.ssh/id_ed25519
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

or

```bash
eval "keychain --eval --agents ssh id_ed25519"
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
  IdentityFile ~/.ssh/id_rsa # or id_ed25519 depending on the encryption type
```

The `AddKeysToAgent` part will tell SSH to use `ssh-agent` to store keys, the `TCPKeepAlive` and `ServerAliveInterval` will make sure my connection to the server doesn't time out if you walk away from tinkering with it for a while, and the `IdentityFile` just says what the default private key should be used for establishing SSH connections.
