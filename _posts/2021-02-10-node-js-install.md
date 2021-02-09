---
layout: post
title: "First-time Node.js development - 1 - Installation and Version Management (NVM, n)"
author: "Logger"
thumbnail: "https://heropy.blog/css/images/vendor_icons/nodejs.png"
tags: 
---


![image](https://heropy.blog/css/images/vendor_icons/nodejs.png)

Node.js is a software platform for server-side development built with a high-performance JavaScript engine developed by Google called V8.

By default, JavaScript runs in a web browser (client-side), which provides all kinds of server-side tools in the Node.js runtime environment for server development with JavaScript.
Therefore, JavaScript can be used to develop both the client side (front-end) and the server side (back-end), so no additional language learning is required for web development.

Node Packaged Manager (npm), Node.js` package ecosystem, is also the world`s largest open source library ecosystem.
When Node.js is installed, npm can be installed and used together.

# Installation

Describes some recommended methods for installing Node.js.

## Direct Installation

Go to the Node.js homepage to receive and install the installation file.

### LTS vs Current

When accessing the Node.js homepage, the installable download buttons are divided into `LTS` and `Current` (current version)

![image](https://heropy.blog/images/screenshot/node-js-home.jpg)

Long Term Supported (LTS) is a long-term stable and reliable version of support that is recommended for most users, focusing on maintenance and security (such as server operations).
The even version (ex. 8.x.x) is the LTS version.

Current (current version) is a version that provides the latest features and focuses on improving the functionality of existing APIs, which is suitable for simple development and testing because updates are frequent and the functionality is likely to change.
The odd version (ex. 9.x.x) is the Current version.

We will install LTS version here.

After installation, you can verify the installation status and version by typing the following in the terminal:
(When Node.js is installed, npm is installed together)

```bash
$ node -v
# v8.9.4
$ npm -v
# 5.6.0

```

## Installing as Package Manager

If you want to install it in more different ways, check to install Node.js as package manager.

### macOS - Homebrew

Install the LTS version using Homebrew, package manager for macOS.
Enter only the version of Major Number.

```bash
$ brew install node@8

```

### Windows - Chocolatey

Install using Chocolatey, package manager for Windows.

```bash
$ choco install nodejs-lts

```

# Version Management

When a new version of Node.js is released, you may need to upgrade the version and downgrade the version for backward compatibility.
We recommend that you install Node.js in a version-manageable form from the time you install it, as changes to the version occur frequently in various cases.
Of course, if versioning is not required yet, you may skip this step.

NVM and n are representative management managers for versioning of Node.js.
NVM and n provide similar functionality.
However, as of February 2018, when this article was written, NVM and n are not yet supported by Windows OS.
(You can use nvm-windows or nodists as a replacement for NVM.)

## NVM

https://github.com/creationix/nvm

To avoid conflicts, it is recommended that you uninstall the previously installed version of Node.js before installing Node Version Manager (NVM).
To install or update Node Version Manager (NVM), you can use the installation script as cURL.

```bash
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash

```

Alternatively, you can install it using Homebrew.

```bash
$ brew install nvm

```

Once installed, `~/.nvm in profiles such as `bash_profile`, `~/.zshrc`, and `~/.profile`.The following script is added to run sh:

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ]
```

If it`s not added, I`ll add it myself.

```bash
'# Install Node Version
$ nvm install <version> # ex> nvm install 8.9.4

# Check the list of installed Node versions
$ nvm ls

# Set Node to Use
$ nvm use <version> # ex> nvm use 8.9.4
$ nvm use <alias> # ex> nvm use default

# Set alias to use
$ nvm alias <alias> <version> # ex> nvm alias test-v 8.9.4

```

Additional Usage

NVM does not support Windows OS, but unofficially nvm-Windows and nodist are available in Windows.

### nvm-windows

https://github.com/coreybutler/nvm-windows

nvm-Windows downloads and installs `nvm-setup.zip` as an installer.

Usage

### nodist

https://github.com/marcelklehr/nodist

The nodist also downloads and installs the installer.

Alternatively, you can install it using Chocolatey.

```bash
$ choco install nodist

```

Usage

## n

n is easier to install because you do not need to uninstall the existing Node.js.
Install global at npm.

```bash
$ npm install -g n

# If you require administrator privileges
$ sudo npm install -g n

```

As expected, it introduces a simple method of use.
If you require administrator privileges (`Error: sudo required`), put `sudo` before it. You may also need it for removal (rm).

```bash
'# Use installed Node version (select from list and Ctrl + c)
$ n

# Check all Node versions for installed versions
$ n ls

# Install Node Version
$ n <version> # ex> n 8.9.4
$n latest # Install the latest version of LTS Node

# Set Node to Remove
$ n rm <version ...> #ex> n rm 8.9.1 8.9.2
$n prune # Remove all versions except the current version

```

Additional Usage

# References

https://nodejs.org/dist/latest-v6.x/docs/api/documentation.html
https://ko.wikipedia.org/wiki/Node.js
https://medium.com/gitignore/nvm-vs-n-f34ebca314ea