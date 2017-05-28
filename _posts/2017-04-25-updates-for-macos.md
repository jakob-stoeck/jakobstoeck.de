---
layout: post
title:  "Updates for macOS"
date:   2017-04-25 14:00:01 +0200
categories:
---

Installing and updating system packages and applications is much easier under macOS using the CLI if you use [homebrew](https://brew.sh) and the [Mac App Store Command Line Interface](https://github.com/mas-cli/mas), then you can just:

update all the system components,

```shell
$ softwareupdate -ia
```

update all the apps installed via the App Store and

```shell
$ mas upgrade
```

update everything installed via brew and cleans up the otherwise always growing cache folder

```shell
$ brew upgrade --cleanup
```

Now you have a completely up-to-date machine. I added this to my .zshrc to have a shell function for it, so you can just type "upgrade" instead of the three commands above:

```shell
upgrade() {
	# updates system updates, mac app store and homebrew packages
	softwareupdate -ia && mas upgrade && brew upgrade --cleanup
}
outdated() {
	softwareupdate -l && mas outdated && brew outdated
}
```
