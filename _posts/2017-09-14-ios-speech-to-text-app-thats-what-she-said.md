---
layout: post
title:  "iOS Speech To Text App - That’s what she said"
date:   2017-09-14 09:30:00 +0200
categories:
---

# That’s what she said

With That’s What She Said you can read the text of any voice message or audio file you receive.

[View on App Store](https://itunes.apple.com/us/app/thats-what-she-said/id1239469302?ls=1&mt=8)

## How it works

You need to send the voice message you have to an iOS action. Depending on the app this opens behind a button called "Share", "Forward" or "Export".

<svg style="float: left; margin-right: 5px" version="1.0" xmlns="http://www.w3.org/2000/svg"
 width="12.000000pt" height="17.000000pt" viewBox="0 0 120.000000 173.000000"
 preserveAspectRatio="xMidYMid meet">
<g transform="translate(0.000000,173.000000) scale(0.100000,-0.100000)"
fill="#000000" stroke="none">
<path d="M468 1593 c-76 -76 -138 -143 -138 -148 0 -6 12 -19 27 -29 l26 -19
90 91 c49 51 91 92 93 92 2 0 4 -202 4 -450 l0 -450 35 0 35 0 0 452 0 453 93
-92 92 -92 27 22 26 22 -136 143 -137 143 -137 -138z"/>
<path d="M0 640 l0 -600 600 0 600 0 0 600 0 600 -220 0 -220 0 0 -40 0 -40
180 0 180 0 0 -520 0 -520 -520 0 -520 0 0 520 0 520 180 0 180 0 0 40 0 40
-220 0 -220 0 0 -600z"/>
</g>
</svg>

Often it’s also this share symbol.

If you tap on it, the iOS actions open where you can choose That’s What She Said:

<img src="/assets/ios-action.png" width="311" alt="iOS action">

The transcribed text is shown after a few seconds as a notification:

<img src="/assets/ios-twss-text.png" width="311" alt="iOS action">

## Supported Apps

Most apps should word since it only needs to support iOS actions. Those appear usually if you tap on "Share", "Forward" or "Export". I tried it succesfully with:

- WhatsApp
- Telegram
- Voice Memos

One notable exception is the Apple Messages app. I couldn’t find any way to share or export a voice message in this app.

## Privacy

Depending on the audio format voice is either sent directly to Google’s (for Opus and Flac) Speech Recognition service or Apple’s (all other audio formats). No other data is transmitted or stored.
