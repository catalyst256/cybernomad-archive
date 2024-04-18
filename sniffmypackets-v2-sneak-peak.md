---
title: "sniffmypackets v2 - Sneak Peak"
date: "2014-09-18"
categories: 
  - "archive"
---

So this week I started the long awaited (well on my part) rewrite of [sniffmypackets](http://sniffmypackets.net/). My initial release was more a voyage of discovery rather than a well thought out application but it did teach me a lot about how to write transforms for Maltego (using Canari Framework) so I felt it was best to start from scratch and just reuse chunks of code where it made sense.

I've also decided to add more functionality that will make using SmP (sniffmypackets) more than just a Maltego based exercise. There are 3 key features I will be adding.

1. Database Support
2. Web Interface
3. Netflow support

The decision to use a database (MongoDB at the moment) was to allow for the database to exist after Maltego is closed. Every pcap will have a unique Session ID which means that not only can you load a pcap into SmP you can (on a different machine and without the original pcap) then rebuild the contents of the pcap based on the Session ID.

The web interface will mean that other people (who may or may not have Maltego) can then view key information about a pcap or you can just use it as another source of reference.

Both the database and web interface will be able to run outside of the machine running Maltego (so you can centralise it) and I might even include Vagrant machines if you don't have the capacity to do that.

It does mean that SmP will need a MongoDB backend to run but trust me it will be worth it.

After a conversation I had a few weeks ago with some very clever people I decided to add Netflow capabilities into SmP to give you even more bang for your buck (so to speak).

Below are some screenshots of what it looks like at the minute.

[![SmP - Overview](http://theitgeekchronicles.files.wordpress.com/2014/09/smp-overview.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/smp-overview.png)

[![SmP - Session Import](http://theitgeekchronicles.files.wordpress.com/2014/09/smp-session-import.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/smp-session-import.png)

[![SmP-DNS-Screenshot](http://theitgeekchronicles.files.wordpress.com/2014/09/smp-dns-screenshot.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2014/09/smp-dns-screenshot.png)
