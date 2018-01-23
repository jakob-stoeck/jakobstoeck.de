---
layout: post
title:  "Updates for macOS"
date:   2017-04-25 14:00:01 +0200
categories:
---

Installing and updating system packages and applications is much easier under macOS using the CLI if you use [homebrew](https://brew.sh) and the [Mac App Store Command Line Interface](https://github.com/mas-cli/mas), then you can just:

update all the system components,

```shell
$ softwareupdate -i
```

update all the apps installed via the App Store and

```shell
$ mas upgrade
```

update everything installed via brew and cleans up the otherwise always growing cache folder

```shell
$ brew upgrade --cleanup
```

Now you have a completely up-to-date machine. I added this with some additional upgrade commands to my .zshrc to have a shell function for it, so I can just type `$ upgrade`:

```shell
upgrade() {
	# updates system updates, mac app store, homebrew packages and ruby gems
	softwareupdate -i
	mas upgrade
	brew upgrade
	brew cleanup -s
	for app in `brew cask outdated --quiet`; do brew cask install --force $app; done
	brew cask cleanup
	gem update --no-document
	gem cleanup
}
outdated() {
	softwareupdate -l
	mas outdated
	brew outdated
	brew cask outdated
	gem outdated
}
```
