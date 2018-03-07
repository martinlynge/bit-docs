---
id: installation
title: Installation
permalink: docs/installation.html
layout: docs
category: Getting Started
next: initializing-bit.html
prev: quick-start.html
# prev: what-is-bit.html
---

Bit can be installed on Mac, Windows or Linux machines, and available through various installation methods.

Once installed, verify the installation by running the following command:

```bash
bit --version
```

## NPM

Install Bit using [NPM](https://www.npmjs.com/package/bit-bin):

```bash
npm install bit-bin --global
```

## Yarn

Install Bit using [Yarn](https://yarnpkg.com/en/package/bit-bin):

```bash
yarn global add bit-bin
```

## macOS

### Homebrew

Bit can be installed via [Homebrew](https://brew.sh) package manager.

```bash
brew install bit
```

## Windows

Bit uses [posix](https://www.npmjs.com/package/posix), which is irrelevant for Windows. When installing Bit on Windows using Yarn or NPM, installing `posix` as a dependency fails, but that's completely fine. Bit will still be installed and will function properly.

<!-- There are two options to install Bit on Windows.

### Download installer

Download the `.msi` installer.

Note - you will need to have Node.js pre-installed.

[Download MSI installer](https://api.bitsrc.io/release/msi/download) -->

### Chocolatey

Bit can be installed via [Chocolatey](https://chocolatey.org).

```bash
choco install bit
```

## Debian/Ubuntu Linux

On Debian or Ubuntu Linux, you can install Bit via our Debian package repository.

Configure it using this command:

```bash
curl https://bitsrc.jfrog.io/bitsrc/api/gpg/key/public | sudo apt-key add -sudo bash -c "echo 'deb http://bitsrc.jfrog.io/bitsrc/bit-deb all stable' >> /etc/apt/sources.list"
```

Then simply install using:

```bash
sudo apt-get update && sudo apt-get install bit
```

## CentOS / Fedora / RHEL

On CentOS, Fedora and RHEL, you can install Bit via our RPM package repository.

```bash
sudo wget http://assets.bitsrc.io/bitsrc.repo -O /etc/yum.repos.d/bitsrc.repo
```

Then simply install using:

```bash
sudo yum install bit
```
