---
title: "Scapy: pcap 2 dns"
date: "2013-10-16"
categories: 
  - "archive"
---

So the second piece of code in my series on the python & Scapy lovefest is another simple bit of code that looks through a pcap file and pulls out some DNS information. The initial thought behind this was making it easy to look for DNS domains that might be "dodgy", i.e. has lots of DNS answer records and a low(ish) TTL.

Now the downside of using Scapy to look at DNS records is that it's very very difficult to pull out all the DNS answer records (the IP's) that might be contained in a DNS Response packet. They are essentially nested in the Scapy packet and I haven't worked out a way to return them all (yet). However fear not my packet warriors there is a DNS ancount field which tells you have many answers exist in the DNS response, so I've used that as a measure of how "dodgy" the domain is.

Again the script can be found on my GitHub repo [HERE](https://github.com/catalyst256/MyJunk):

To run the script you simply need to do this:

`./pcap-dns.py pcapfile`

I'm not going to post the code on the post this time (formatting is a bitch) but you should get something like this back (I used a fastflux pcap file for the demo).

[![pcap2dns](http://theitgeekchronicles.files.wordpress.com/2013/10/pcap2dns.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2013/10/pcap2dns.png)

The red coloured lines are DNS responses that have over 4 IP addresses in the response, you can change this in the code if you want but it does give you a good idea of what is going on. I also throw back the TTL because if it is something like [Fast Flux](http://en.wikipedia.org/wiki/Fast_flux) it would probably have a high number of ancount records and a low TTL.

Simples..
