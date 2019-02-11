---
layout: post
title:  "Stuff I used in January 2019"
date:   2019-01-02 23:08:00 +0200
categories:
excerpt: Quick recap of noteworthy tech stuff I used
---

Quick recap of noteworthy tech stuff I used:

## 1. Pipe `tcpdump` from remote server to local Wireshark

This will show all the traffic from `remote` on your local Wireshark application. `brew install wireshark` before if you haven’t done, yet:

```shell
ssh username@remote 'tcpdump -s0 -nn -w - not port 22' | wireshark -k -i -
```

See the `not port 22` which makes sure to not track your ssh traffic your sending from the remote to the local server.

## 2. CSP: Content Security Policy

A HTTP Header to whitelist script locations so you mitigate <abbr title="Cross-site scripting">XSS</abbr> attacks. See the [<abbr title="Mozilla Developer Network">MDN</abbr> Website for details.](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP), e.g. if you want all your content come from your domain, excluding subdomains, you should send this <abbr title="Hypertext Transfer Protocol">HTTP</abbr> header: `Content-Security-Policy: default-src 'self'`.

## 3. Format JavaScript Objects as JSON with jq



`jq` can only format JSON but not plain JavaScript objects. To convert the latter to the former I used "relaxed JSON" (`rjson`) like this:

```shell
brew install rjson jq
rjson <(./returnsJavaScriptCmd) | jq
```

## 4. Draw diagrams with yEd

It seems that [yEd](https://www.yworks.com/products/yed) is the best editor for technical diagrams out there. If not dot files go a long way and there is an easy "[GraphViz](https://www.graphviz.org/) for the poor", i.e. viewing dot files without the graphviz app. It is using the excellent [entr](http://eradman.com/entrproject/) to run automatically on file change:

```shell
brew install entr dot
echo myfile.dot | entr -c dot -Tpdf -o /tmp/dot_out.pdf /_
```

## 5. Flutter

Write native Android apps in Dart.

## 6. PostgREST

Serve a RESTful API from any database with the great [PostgREST.](http://postgrest.org/) Death to the <abbr title="Object Relational Mapping">ORM</abbr> and all its business code which is duplicating database structures. Here you use all the power of a great database to build up a <abbr title="Create Read Update">CRUD</abbr> <abbr title="Apllication Programming Interface">API</abbr> including authorization. Declarative programming <abbr title="For the win">ftw</abbr>!

## 7. GreenSock Animation Platform

<abbr title="GreenSock Animation Platform">GSAP</abbr> is professional-grade animation for JavaScript canvas and looks [amazing.](https://greensock.com/)

## 8. Jaeger

Tracing in distributed systems with [jaeger.](https://www.jaegertracing.io/)

## 9. Update your private key

If your private key is still in an old or unsecure format, you can update it with:

```shell
ssh-keygen -p -o -f PRIVATE_KEY
```

## 10. Business Process Modelling Language

Model your business process in <abbr title="Business Process Modelling Language">BPML</abbr> instead of writing business logic in code. Old-school tools like [Activiti](https://www.activiti.org/) or [jPBM](https://www.jbpm.org/) support them and there are also fancy new <strike>unstable</strike> nodejs packages for it. This makes it easy for your stakeholders to define and change business processes without you having to change that in your code.
