# Ubuntu 20.04

## Git

### Installation

```sh
$ sudo add-apt-repository ppa:git-core/ppa

$ sudo apt update

$ sudo apt install git
```

## Visual Studio Code

### Installation

```sh
$ sudo snap install code --classic
```

### Setting

```sh
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

## IntelliJ IDEA

### Installation

```sh
$ sudo snap install intellij-idea-ultimate --classic
```
