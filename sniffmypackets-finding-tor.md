---
title: "sniffMyPackets: Finding Tor"
date: "2013-04-08"
categories: 
  - "archive"
---

I don't normally do short random posts but I figure once in a while won't hurt.

So I've been busy working on new transforms for my Maltego pcap analysis package and things are moving along nicely. Part of my process is making notes on things I think would be cool to see and then working my way through the list.

Over the weekend I added "Tor Traffic" to my list, I know most of the traffic is encrypted so wasn't sure if I could get an end result from it but figure it was worth a look.

Anyway I 've thrown together some Scapy/Python code (soon to be a transform if it's right) that I think will highlight Tor traffic in a pcap file. Now this is just a work in progress so let me know if I've miles off the mark (so to speak).

I created a pcap file by stopping the Tor service on my copy of Backtrack and then starting it again while capturing some packets. I've also tested it on another pcap file from the internet with some Tor bot traffic and the results are similar.

Looking through the pcap file I noticed some "strange" entries during the SSL handshake that lead me to my PoC code (no I didn't Google first to see if it already existed).

During the SSL handshake the "**Client Hello**" packet includes a **Server Name** record which in a "normal" handshake might be similar to www.google.com however with a Tor SSL handshake its something like "**www.wth7pbtqsw6.com**", which if you ping doesn't actually exist.

The other thing to note is that the SSL server name doesn't have a corresponding DNS query, in fact for all the packets in the pcap file there are no DNS queries/responses which is another way to narrow down possible Tor traffic.

The sniffMyPackets transform can be found [HERE](https://github.com/catalyst256/sniffMyPackets/blob/master/src/sniffMyPackets/transforms/findtor.py):

So basically my python code reads a pcap file and looks for any TCP packet with a payload, that has www.xxxxxx in it. It then pulls out the key information (src ip, dst ip, sport, dport and www. value). and just displays it as neat little lines.

The code is below:

`#!/usr/bin/env python`

import logging, sys logging.getLogger("scapy.runtime").setLevel(logging.ERROR) from scapy.all import \*

pcap = sys.argv\[1\]

pkts = rdpcap(pcap)

for x in pkts: if x.haslayer(TCP) and x.haslayer(Raw): if 'www.' in x.getlayer(Raw).load: for s in re.finditer('www.\\w_.\\w_', str(x)): dnsrec = s.group() srcip = x.getlayer(IP).src dstip = x.getlayer(IP).dst sport = x.getlayer(TCP).sport dport = x.getlayer(TCP).dport ipaddr = srcip, dstip, sport, dport, dnsrec print ipaddr

If I run this against my pcap file I get these results:

`root@bt:~# ./lookfortor.py /root/pcaps/tor-startup.pcap src: 192.168.1.66 dst: 89.160.29.195 sport: 40651 dport: 9001 dnsrec: www.qqsuvxwbs.com src: 89.160.29.195 dst: 192.168.1.66 sport: 9001 dport: 40651 dnsrec: www.vlkkj3kgxh56ibujher.net0 src: 192.168.1.66 dst: 86.59.119.83 sport: 48459 dport: 443 dnsrec: www.buukx57zhxo2ujugeevlveb.com src: 86.59.119.83 dst: 192.168.1.66 sport: 443 dport: 48459 dnsrec: www.yhlkhxjmzg3.net0 src: 192.168.1.66 dst: 38.229.70.42 sport: 44829 dport: 443 dnsrec: www.wth7pbtqsw6.com src: 38.229.70.42 dst: 192.168.1.66 sport: 443 dport: 44829 dnsrec: www.uvk2wwbbvwpqpkux.net0 root@bt:~#`

What you will notice is that the reply packet has a different dnsrec which always has a 0 (zero) on the end..

I've tested this on a few pcap files and the results look consistent. Let me know what you think.
