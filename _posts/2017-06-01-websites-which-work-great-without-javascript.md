---
layout: post
title:  "Websites which work great without JavaScript"
date:   2017-06-01 13:35:00 +0200
categories:
---

Contrary to popular belief, the web still works quite well without JavaScript:

## These sites work great with all essential functionality

- [Amazon.com](https://www.amazon.com/)
- [Baidu.com](https://Baidu.com/)
- [Blogger.com](https://Blogger.com/)
- [DuckDuckGo.com](https://DuckDuckGo.com/)
- [eBay.com](https://eBay.com/)
- [Facebook.com](https://Facebook.com/) (redirects to mobile version)
- [Google.com](https://Google.com/)
- [Medium.com](https://Medium.com/)
- [Mozilla.org](https://Mozilla.org/)
- [news.ycombinator.com](https://news.ycombinator.com/)
- [Reddit.com](https://Reddit.com/) (Desktop version)
- [StackOverflow.com](https://StackOverflow.com/)
- [Twitter.com](https://Twitter.com/)
- [Wikipedia.org](https://Wikipedia.org/)
- [WordPress.com](https://WordPress.com/)
- [Xkcd.com](https://Xkcd.com/)
- [Yahoo.com](https://Yahoo.com/)

## These work with some functionality missing

- [bing.com](https://bing.com/) (no images, no login)
- [espn.com](https://espn.com/) (some images missing)
- [GitHub.com](https://GitHub.com/) (read-only browsing works, opening PRs and the like not)
- [GitLab.com](https://GitLab.com/) (read-only browsing works, drop down menus are not)
- [meetup.com](https://meetup.com/) (no RSVP)
- [tumblr.com](https://tumblr.com/) (blogs work, the search does not)
- [WashingtonPost.com](https://WashingtonPost.com/) (some images missing)

## Do not work

- any video except gifs (YouTube.com, Twitch.tv, even native HTML5 videos need JavaScript for the play event. There are applications which support YouTube URLs though, like VLC: `vlc 'https:// www.youtube.com/watch?v=somevideoid'`, though)
- [instagram.com](https://instagram.com/)
- [linkedin.com](https://linkedin.com/)
- [maps.google.com](https://maps.google.com/)
- [Netflix.com](https://Netflix.com/)
- [Reddit.com](https://Reddit.com/) (mobile version. alternative: use the desktop version)
- [taobao.com](https://taobao.com/)
- [ted.com](https://ted.com/) (video is not playing, download button does not work either, but that could be fixed easily)
- [translate.google.com](https://translate.google.com/) (error 403)

# Why disable JavaScript?

## Faster browsing

Loading times are much lower since JavaScript files are not requested. Rendering times are also faster because no scripts are executed during this phase. Most pages are finished in under one second.

## Less CPU usage

Besides the initial loading, many sites are continuously executing scripts which put load on the CPU. Badly written pages even more so.

## More battery on mobile

Mobile surfing holds up much longer.

## Less intrusive animations

No "subscribe to our newsletter" popups, no scrolling-based "this might interest you" notifications. No changing of scrolling behavior.

# How to disable JavaScript

Go cold turkey:

- Safari: Develop > Disable JavaScript
- iOS: Settings > Safari > Advanced > JavaScript
- Chrome: Preferences > Show Advanced Settings … > Content Settings … > Do not allow any sites to run JavaScript
- Firefox: about:config > javascript.enabled = false

Pro Tips:

- For Safari under macOS you can set a Keyboard Shortcut to toggle JavaScript under System Preferences, e.g. Cmd-Shift-J
- For Chrome you can add exceptions to sites you like to always run JavaScript

Alternatively use an extension like NoScript.


If you want to add a site or change something [just edit this page.](https://github.com/jakob-stoeck/jakobstoeck.de/edit/master/_posts/2017-06-01-websites-which-work-great-without-javascript.md) _With_ JavaScript since GitHubs commit button does not work without it. If anyone from GitHub is reading this, it would be great if you could fix this 😉
