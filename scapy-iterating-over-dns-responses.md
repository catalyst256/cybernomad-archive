---
title: "Scapy - Iterating over DNS Responses"
date: "2014-05-12"
categories: 
  - "archive"
---

So while doing my Scapy Workshop at BSides London the other week, I stated that iterating over DNS response records with Scapy is a bit of a ball ache. Well I will be honest, I was kind of wrong. It's not that difficult it's just not that pretty.

This is an example of a DNS response (Answer) packet when running a dig against www.google.com

[![Screen Shot 2014-05-12 at 08.14.07](http://theitgeekchronicles.files.wordpress.com/2014/05/screen-shot-2014-05-12-at-08-14-07.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2014/05/screen-shot-2014-05-12-at-08-14-07.png)

You will see that there are 5 DNSRR layers in the packet, now when you ask Scapy to return the rdata for those layers you will only get the first one (in the Scapy code below pkts\[1\] refers to the second packet in the pcap which is the response packet).

pkts\[1\]\[DNSRR\].rdata '173.194.41.148'

In order to get the rest, you need to iterate over the additional layers.

pkts\[1\]\[5\].rdata '173.194.41.148' pkts\[1\]\[6\].rdata '173.194.41.144'

In the example above the \[5\] and \[6\] are the layer "numbers" so to make that a bit easier to understand.

pkts\[1\]\[0\] = Ether pkts\[1\]\[1\] = IP pkts\[1\]\[2\] = UDP pkts\[1\]\[3\] = DNS pkts\[1\]\[4\] = DNSQR pkts\[1\]\[5-9\] = DNSRR

So the code below is a quick way to iterate over the DNS responses by using the ancount field to determine the number of responses and then working backwards through the layers to show all the values.

`#!/usr/bin/env python`

from scapy.all import \*

pcap = 'dns.pcap' pkts = rdpcap(pcap)

for p in pkts: if p.haslayer(DNSRR): a\_count = p\[DNS\].ancount i = a\_count + 4 while i > 4: print p\[0\]\[i\].rdata, p\[0\]\[i\].rrname i -= 1

So there you go, quick and dirty Scapy/Python code.

Enjoy!!
