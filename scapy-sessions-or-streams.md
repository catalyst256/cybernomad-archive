---
title: "Scapy: Sessions (or Streams)"
date: "2014-12-02"
categories: 
  - "archive"
---

I'm in process of rewriting sniffMyPackets version 2 (and yes I'm actually doing it this time), part of work involves doing a complete rewrite of the Scapy based code that I used in the original version. This isn't to say anything is wrong with the original version but my coding "abilities" has changed so I wanted to make it more streamline and hopefully more modular.

In the original sniffMyPackets I used tshark to extract TCP and UDP streams, which wasn't the ideal solution however it worked so.. This time around I'm hoping to cut out using tshark completely so I've looking at what else Scapy can do. There is a "hidden" (well I didn't know it was there) function called Sessions, this takes a pcap file and breaks it down into separate sessions (or streams). The plus side of this is that it's actually really quick and doesn't limit itself to just TCP & UDP based protocols.

I'm still playing with the function at the moment but I'm hoping that I can use this for sniffMyPackets, so I thought I would share my findings with you.

First off lets load a pcap file into Scapy.

`>>> a = rdpcap('blogexample.pcap')`

This is just a sample pcap file with a ICMP ping, DNS lookup and SSL connection to google.co.uk.

If you want to check the pcap has loaded correctly you can now just run.

`>>> a.summary()`

Cool so lets load the packets into a new variable to hold each the sessions.

`>>> s = a.sessions()`

This loads the packets from the pcap file into the new variable as a dictionary object, which I confirmed by during.

`>>> print type(s) <type 'dict'>`

Now lets get a look at what we have got to work with now.

`>>> s {'UDP 192.168.11.228:21893 > 208.67.222.222:53': ...SNIP...` That's just the first session object from the pcap and it contains two parts (as required for a dictionary object). The first is a summary of the traffic, it contains the protocol, source IP address and port (split by a colon) and then the destination IP address and port (again split by a colon). The second part of the dictionary object is the actual packets that make up the session.

Lets have a look at how to make use of the first part of the dictionary object (useful if you want a summary of the sessions a pcap file).

`>>> for k, v in s.iteritems():  `

> `>> print k >> UDP 192.168.11.228:21893 > 208.67.222.222:53 UDP 192.168.11.228:44710 > 208.67.222.222:53 TCP 192.168.11.228:53482 > 208.180.20.97:443 UDP 208.67.222.222:53 > 192.168.11.228:12321 ...SNIP...`

This is a nice and easy iteration of the dictionary object to get all the key within it (the summary details). You can of course slice and dice that as you see fit to make it more presentable. The other part of the sessions dictionary is the actual packets, using a similar method we can iterate through the sessions dictionary and pull out all the raw packets.

`>>> for k, v in s.iteritems():  `

> `>> print v >>`

This will output a whole dump of all the packets for all of the sessions which isn't that great. Lets see if we can make it a bit more friendly on the eye.

`>>> for k, v in s.iteritems():  `

> `>> print k >> print v.summary() >> ...SNIP... TCP 208.180.20.97:443 > 192.168.11.228:53482 Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 SA Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 A Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 A / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 A / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 PA / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 PA / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 PA / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 A Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 PA / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 A Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 PA / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 PA / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 A Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 PA / Raw Ether / IP / TCP 208.180.20.97:https > 192.168.11.228:53482 PA / Raw ...SNIP...`

Now isn't that much better, as you can see from the snippet above the session it's captured starts at the SYN/ACK flag (probably because I started my capture after the SYN was sent) so in reality (and with a bit more testing) it looks like the Scapy Sessions function does allow for extracting TCP streams (well all streams really).

Lets have a go at something else.

`>>> for k, v in s.iteritems():  `

> `>> print v.time >> AttributeError: 'list' object has no attribute 'time' >>>` Oh well I was hoping that I could get the time stamp for each packet but it seems that you need to do some extra work first.

`>>> for k, v in s.iteritems():  `

> `>> for p in v: >> print p.time, k >> 1417505523.55 UDP 192.168.11.228:21893 > 208.67.222.222:53 1417505522.45 UDP 192.168.11.228:44710 > 208.67.222.222:53`

Oh that works much better, because the packets in the second half of the dictionary are a list object we need to iterate over them to get access to any of the values within them (such as the packet time).

So there you go, I'm off to see what else I can do with this..

Enjoy.
