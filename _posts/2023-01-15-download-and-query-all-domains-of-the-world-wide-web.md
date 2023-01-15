---
layout: post
title:  "Download and query all domains of the world-wide web"
date:   2023-01-15 09:31:00 +0800
categories:
excerpt: Query all top level domains (TLD) locally
---

I was wondering whether I could get a list of all world wide web domains on my local laptop and query them, like a phone book. As it turns out, that is straight-forward: You can download all top-level domains (TLD) zone server zonefiles, then use local tools to search through the less than 10 GB gzipped files.

## How to download

Since domains are stored online in domain name system (DNS) servers that are reachable by the public, you could use the DNS protocol to request for the full list of domains of each DNS server that is responsible for a top-level domain (like .com). But for security reasons, you cannot request a zonefile from an authoritative DNS server anymore as this would enable an [amplification denial-of-service attack](https://en.wikipedia.org/wiki/Denial-of-service_attack#Amplification).

That is why the Internet Corporation for Assigned Names and Numbers (ICANN) provided a Centralized Zone Data Service, where any interested party can request and subsequently download nearly all zone files. For the vast majority of the 1,000+ TLD you don’t need a better reason than "for research purposes", whereas some of the more obscure ones request for more information.

To download all zonefiles for the as of writing of this post 266,372,902 domains (easy to remember: slightly less domains than `c`, the speed of light):

1. Go to <https://czds.icann.org/> and register
2. Go to <https://github.com/icann/czds-api-client-python>, input your credentials as mentioned in the readme, install dependencies and download:

```bash
$ cd czds-api-client-python
$ cp config{.sample,}.json
# update config.json with your credentials
$ pipenv install requests
$ pipenv run python download.py
```

## How to query

Now, you will have more than 1,000 gzipped text files with all domains. The files are tab-separated. DNS has entry types that mark different purposes: mail servers, name servers (what your browser uses), signatures, etc. 

For example, the first 5 lines of the `.pizza` TLD look like this:

```bash
$ zless pizza.txt.gz | head -n5
pizza.	3600	in	soa	v0n0.nic.pizza. hostmaster.donuts.email. 1672103559 7200 900 1209600 3600
0.pizza.	3600	in	ds	27676 8 2 95C9CB61DEF5016F2905C7478044E011AE3C041AEB31E5BC80C11DEE7FC05142
0.pizza.	3600	in	ns	ns-cloud-d1.googledomains.com.
0.pizza.	3600	in	ns	ns-cloud-d2.googledomains.com.
0.pizza.	3600	in	ns	ns-cloud-d3.googledomains.com.
```

For the reason of this project I’m interested in the name servers which are marked with `ns` in the fourth column. You see that for the first lines of pizza.txt.gz that is the domain http://0.pizza/ (Which is defunct. What did the owner want to do with that domain? Do they sell pizzas in the shape of a zero with a hole in the middle? We will never know.).

To get all domains from a name server (the ones your web browser would use), we filter on the fourth column, remove duplicates, and send them to [fzf](https://github.com/junegunn/fzf) (on mac: `$ brew install fzf`) for fuzzy searching capabilities:

```bash
gzcat *.txt.gz | awk -F '\t' '$4=="ns" { print $1 }' | uniq | fzf
```

Turns out this is fast enough to checkout the data.

Searching through all domains worldwide instantly is pretty fun! There are so many fun, surprising and head-scratching domain names out there. I hope you find some which excite you!


## Some observations

#### People use DNS for much more than I thought.

Especially public signatures, see an example from `.pizza`:

```bash
$ gzcat pizza.txt.gz | awk -F '\t' '{ print $4 }' | sort | uniq 
a
aaaa
dnskey
ds
ns
nsec3
nsec3param
rrsig
soa
```

#### There are so many TLDs!

A bit less than 2,000 TLDs, with even local variants like `.vermögensberater` (financial advisor). You can lookup the history and raison d'être of each TLD on <https://icannwiki.org/>.

#### The first domain was `.no`

#### Limitations

The zone files do not include subdomains. Some TLDs use third level domain names, like `.co.no`. These subdomains are not on the TLD zone server either, and hence not part of this list.
Non-ascii domains are in puny code, and not translated. You would need to convert either your search or the index with something like <https://pypi.org/project/idna/> or <https://docs.rs/idna/latest/idna/>.
