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

### Clients

We encourage you to use CLI client for Git in your terminal but there are also some other clients that you can use implemented in your IDE. Some of them are:

- [GitKraken](https://www.gitkraken.com/)
- [GitHub Desktop](https://desktop.github.com/)
- [SourceTree](https://www.sourcetreeapp.com/)
