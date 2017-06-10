---
layout: post
title:  "How to encrypt RTMP (or anything really) over SSL/TLS"
date:   2017-05-16 15:36:15 +0200
categories:
---
You want to communicate over a secure channel but your client application doesn’t support it? That’s a job for a transparent SSL proxy.

This articles describes on how to use stunnel as a transparent proxy to encrypt communication. With a transparent proxy you do not need to change the client application.

# Problem

You want to send data securely between two computers over the internet but your sender application does not support SSL.

# Example

You want to stream a video with [ffmpeg](https://ffmpeg.org) over RTMP to an RTMP server like [Wowza](https://www.wowza.com) or [nginx with an rtmp module](https://github.com/arut/nginx-rtmp-module). While ffmpeg supports RTMP it does not support the SSL-encrypted version RTMPS.

We use [stunnel](https://www.stunnel.org/) to wrap the RTMP stream with SSL/TLS ("SSL proxy") and deliver it to the RTMP server. The server uses stunnel again to decrypt the RTMP stream.

<svg width="440px" height="81px" style="background-color:rgb(255, 255, 255)" version="1.1" xmlns="http://www.w3.org/2000/svg">
 <g transform="translate(.5 .5)">
  <path d="m100 0h40l20 40-20 40h-40l-20-40z" fill="#f5f5f5" pointer-events="none" stroke="#666" stroke-dasharray="3 3" stroke-miterlimit="10"/>
  <g transform="translate(105.5,26.5)">
   <switch>
    <foreignObject width="29" height="26" overflow="visible" pointer-events="all" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility">
     <div color="rgb(0, 0, 0)" display="inline-block" font-family="Helvetica" font-size="12px" text-align="center" style="line-height:1.2;vertical-align:top;white-space:nowrap;width:30px;word-wrap:normal" xmlns="http://www.w3.org/1999/xhtml">
      <div display="inline-block" text-align="inherit" text-decoration="inherit" xmlns="http://www.w3.org/1999/xhtml">SSL
       <div>proxy</div>
      </div>
     </div>
    </foreignObject>
    <text x="15" y="19" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">[Not supported by viewer]</text>
   </switch>
  </g>
  <g transform="translate(11.5,19.5)">
   <switch>
    <foreignObject width="37" height="40" overflow="visible" pointer-events="all" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility">
     <div color="rgb(0, 0, 0)" display="inline-block" font-family="Helvetica" font-size="12px" text-align="center" style="line-height:1.2;vertical-align:top;white-space:nowrap;width:38px;word-wrap:normal" xmlns="http://www.w3.org/1999/xhtml">
      <div display="inline-block" text-align="inherit" text-decoration="inherit" xmlns="http://www.w3.org/1999/xhtml">sender
       <div>e.g.</div>
       <div>ffmpeg</div>
      </div>
     </div>
    </foreignObject>
    <text x="19" y="26" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">[Not supported by viewer]</text>
   </switch>
  </g>
  <g transform="translate(344.5,19.5)">
   <switch>
    <foreignObject width="70" height="40" overflow="visible" pointer-events="all" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility">
     <div color="rgb(0, 0, 0)" display="inline-block" font-family="Helvetica" font-size="12px" text-align="center" style="line-height:1.2;vertical-align:top;white-space:nowrap;width:71px;word-wrap:normal" xmlns="http://www.w3.org/1999/xhtml">
      <div display="inline-block" text-align="inherit" text-decoration="inherit" xmlns="http://www.w3.org/1999/xhtml">receiver
       <div>e.g.</div>
       <div>RTMP server</div>
      </div>
     </div>
    </foreignObject>
    <text x="35" y="26" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">[Not supported by viewer]</text>
   </switch>
  </g>
  <path d="m306.37 40h27.26" fill="none" pointer-events="none" stroke="#000" stroke-miterlimit="10"/>
  <path d="m301.12 40l7-3.5-1.75 3.5 1.75 3.5z" pointer-events="none" stroke="#000" stroke-miterlimit="10"/>
  <path d="m338.88 40l-7 3.5 1.75-3.5-1.75-3.5z" pointer-events="none" stroke="#000" stroke-miterlimit="10"/>
  <path d="m210 20c-24 0-30 20-10.8 24-19.2 8.8 2.4 28 18 20 10.8 16 46.8 16 58.8 0 24 0 24-16 9-24 15-16-9-32-30-24-15-12-39-12-45 4z" fill="#fff" pointer-events="none" stroke="#000" stroke-miterlimit="10"/>
  <g transform="translate(219.5,33.5)">
   <switch>
    <foreignObject width="41" height="12" overflow="visible" pointer-events="all" requiredFeatures="http://www.w3.org/TR/SVG11/feature#Extensibility">
     <div color="rgb(0, 0, 0)" display="inline-block" font-family="Helvetica" font-size="12px" text-align="center" style="line-height:1.2;vertical-align:top;white-space:nowrap;width:42px;word-wrap:normal" xmlns="http://www.w3.org/1999/xhtml">
      <div display="inline-block" text-align="inherit" text-decoration="inherit" xmlns="http://www.w3.org/1999/xhtml">Internet</div>
     </div>
    </foreignObject>
    <text x="21" y="12" fill="#000000" font-family="Helvetica" font-size="12px" text-anchor="middle">Internet</text>
   </switch>
  </g>
  <path d="m66.37 40h107.26" fill="none" pointer-events="none" stroke="#000" stroke-miterlimit="10"/>
  <path d="m61.12 40l7-3.5-1.75 3.5 1.75 3.5z" pointer-events="none" stroke="#000" stroke-miterlimit="10"/>
  <path d="m178.88 40l-7 3.5 1.75-3.5-1.75-3.5z" pointer-events="none" stroke="#000" stroke-miterlimit="10"/>
 </g>
</svg>

Now the sender acts as if it supports encryption and decryption. In the case of ffmpeg it would send an encrypted stream when the SSL proxy is active and an unencrypted one when the SSL proxy is inactive:

```shell
$ ffmpeg -re -i "~/some.mp3" -acodec copy -f flv 'rtmp://www.example.org:443'
```

# Solution

The needed steps are:

1. Install stunnel on a server called gateway
2. Route the sender traffic to the gateway without changing the receiver destination
3. Configure stunnel to re-route the traffic to the receiver address after SSL-wrapping it

## 1. Install stunnel on a server

Stunnel runs on all popular operating systems. To install it use your package manager, e.g. on macOS `brew install stunnel`. Stunnel accepts an incoming connection on a specified port, encrypts it and send it to another specified address.

The default configuration location is `/etc/stunnel/stunnel.conf` (or `$(brew --prefix)/etc/stunnel/stunnel.conf` on macOS). Change its content to

```config
# /etc/stunnel/stunnel.conf
foreground = yes
debug = 5

[my-encryption-service]
client = yes
accept = 1234
connect = www.example.org:443
```

Stunnel now listens to traffic on port 1234, encrypts this traffic and routes it to www.example.org on port 443. For further explanation of the config parameters check `$ man stunnel`. You can test the configuration by starting stunnel and on the same machine connecting to port 1234 (in another terminal window since stunnel is set to `foreground = yes`):

```
$ sudo stunnel
$ echo "hi" | nc 127.0.0.1 1234
```

The stunnel log should then show that you successfully connected to the remote server over a secure connection:

```
[…]
LOG5[0]: Service [my-encryption-service] accepted connection from 127.0.0.1:53870
LOG5[0]: s_connect: connected 93.184.216.34:443
LOG5[0]: Service [my-encryption-service] connected remote server from 192.168.0.1:53871
LOG5[0]: Connection closed: 3 byte(s) sent to TLS, 0 byte(s) sent to socket
```

You could stop here if the destination addresses were known beforehand and could be written in the config. In our example that’s not the case so proceed to step 2:

## 2. Route the sender traffic to the gateway without changing the receiver destination

Now that stunnel works, all the to-be-encrypted traffic needs to be directed to the stunnel port transparently, i.e. without the sender knowing that its traffic gets redirected. This can be done by setting the stunnel server IP as a gateway for the client.

On the sender: Setting a gateway or router IP is usually done in the network configuration. Depending on the client application or operating system that is done in different ways. Search for "set default gateway" and add the operating system your sender runs on if you’re not sure. Then set the "gateway" or "router" IP to the stunnel server from step 1. Now all the traffic which is sent from the client is routed to the gateway address.

On the gateway: The gateway needs to be configured to not drop those packets but to route them to their destination. This is called "IP Forwarding" and should be enabled. Under Linux that can be done with `$ echo 1 > /proc/sys/net/ipv4/ip_forward`. Search online for other operating systems.

Now the client computer should still be able to normally access the network but all its traffic is routed over the gateway before accessing the internet. Which enables us to do the last step:

## 3. Configure stunnel to re-route the traffic to its original destination after SSL-wrapping it

Instead of hard-coding a destination like we did in step 1, change the stunnel configuration to read like this:

```config
# /etc/stunnel/stunnel.conf
foreground = yes
debug = 5

[my-encryption-service]
client = yes
accept = 443
transparent = destination
```

The `accept` port should be equal to the port on the receiver side and we do not have a hard-coded destination anymore but instead use the original destination.

On the gateway: Route the incoming traffic which is directed to port 443 to the local port 443 of stunnel. To do this you need to know how your network is named (look at `$ ifconfig`, in this example I use "eth0" and a Linux gateway):

```shell
$ iptables -I INPUT -i eth0 -p tcp --dport 443 -j ACCEPT
$ iptables -t nat -I PREROUTING -p tcp --dport 443 -i eth0 -j DNAT --to-destination [youriphere]:443
```

### Make it secure

Now you should be able to connect to any service with your sender over an encrypted connection as long as you use the configured port (443 in our example) in the destination address.

To prevent man in the middle attacks, tell stunnel which certificates to accept by concatenating all the valid certificates in a file and add this to the config:

```config
# /etc/stunnel/stunnel.conf
verify = 4
CAfile = /etc/stunnel/your_certificates.crt
```

Ask the receiver maintainer which certificates are valid or have a look yourself at the current certificate chain with this command:

```shell
$ openssl s_client -showcerts -connect www.example.org:443 </dev/null
```

# Limitations

If you need [SNI](https://en.wikipedia.org/wiki/Server_Name_Indication) this approach might not work with all protocols without additional work. The reason is that for SNI you need the destination domain which is written inside the application protocol and must be read by stunnel. But stunnel does not understand all protocols (it normally doesn’t need to for just encrypting it). It does understand some protocols, though, like imap, http, proxy, smtp and others.

The [performance of stunnel](https://www.stunnel.org/perf.html) is nice, but you need to pay attention to some settings like ulimit and which ciphers you use.
