# Ubuntu 20.04

Table of Content

- [Git](#git)
- [Node.js](#node.js)
- [Deno](#deno)
- [Postman](#postman)
- [Visual Studio Code](#visual-studio-code)
- [Docker](#docker)

## [Git](https://git-scm.com/download/linux)

```sh
$ sudo add-apt-repository ppa:git-core/ppa

$ sudo apt update

$ sudo apt install git
```

## Node.js

### [nvm](https://github.com/nvm-sh/nvm)

```sh
$ wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

### [Node.js LTS version](https://nodejs.org/en/download/)

```sh
$ nvm install 12.18.0
```

## [Deno](https://deno.land/)

```sh
$ curl -fsSL https://deno.land/x/install/install.sh | sh

Deno was installed successfully to $HOME/.deno/bin/deno
Manually add the directory to your $HOME/.bashrc (or similar)
  export DENO_INSTALL="$HOME/.deno"
  export PATH="$DENO_INSTALL/bin:$PATH"
```

## [Postman](https://www.postman.com/)

```sh
$ sudo snap install postman

# remove
# $ sudo snap remove postman
```

## Visual Studio Code

### Installation

```bash
$ sudo snap install code --classic
```

### Setting

```bash
$ git config --global core.editor "code --wait"

$ git config --global -e
```

`.gitconfig`

```
[core]
	editor = code --wait
[user]
	email = 18248972+junlico@users.noreply.github.com
	name = junlico
[alias]
	br = branch
	ci = commit
	co = checkout
	st = status
	last = log -1 HEAD
```

## [Docker](https://docs.docker.com/engine/install/ubuntu/#prerequisites)

```sh
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt-get update
$ sudo apt-get install docker-ce docker-ce-cli containerd.io

# uninstall
$ sudo apt-get purge docker-ce docker-ce-cli containerd.io
$ sudo rm -rf /var/lib/docker
```
