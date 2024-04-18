---
title: "sniffMyPackets - The rebirth"
date: "2013-07-31"
categories: 
  - "archive"
---

Dramatic title I know, but in a sense it's true. You may be aware that I started writing Maltego transforms for pcap analysis using the awesome [Canari Framework](https://www.canariproject.com/). At the time it was a mini project where every time I thought of something "interesting" to find a pcap file I wrote a transform to find it (or my interpretation of it). I have no idea how many people make use of it or if it meets their requirements,  I wrote them for my own personal development and just wanted to share.

A few months on and work slowed on the project, mostly because I've had a busy couple of months with courses, study and some mini adventures along the way. Last week I decided that it was time to focus on the project and give it an overhaul (or rebirth), since then I've been working on making it more fluid in terms of the output and the order in which you run transforms. I also decided that although I love Scapy, it's not always the best tool for the job so I've also started to use tshark a lot more.

My recent experience on the **SANS SEC503** course has helped shaped my thought processes behind the way analysts view packet captures, the [sniffMyPackets](https://github.com/catalyst256/sniffMyPackets) transforms needed to align to that to make it more useful and a bit less chaotic (try taking a trip in my mind sometime).

This morning I push a refresh up to my Github repo with some key changes, these were:

- New transforms (more on that in a bit)
- New Maltego machine (called "Release the packets")
- New naming convention

The most important change has been the naming convention, where before everything had a random name based on its function I've now assigned each transform a level, which I hope will make it easier to use.

Level 0 (refered to as **L0** in the transform name) - Are all transforms that you would run against a pcap file to get some basic information from them or are "general use transforms". For example when you add your pcap file into Maltego you can run the "**L0 - Prepare pcap for use**" transform that performs the following functions:

Create a temp folder for storing output from the pcap (streams, rebuilt files etc.) rather than creating a separate folder for everything. SHA1 & MD5 hash the file (so you have these if needed). Convert the pcap file to libpcap format so that Scapy doesn't get upset about "bad magic".

Other transforms at this level include, opening pcap file in wireshark, output pcap file info (number of packets, duration etc.). We then move onto Level 1 (**L1**) transforms which allow you to extract the TCP and UDP (UDP streams aren't as easy to pull out as you would think) streams out of the pcap file and store them as separate pcap files.

From there we move onto Level 2 (**L2**) transforms that find all the "talkers" and "conversations" in that pcap file so you can start to get a sense of the machines involved.

Level 3 (**L3**) transforms are some basic checks, looking for things like DNS requests, rebuilding files and building a GeoIP map of all the IP addresses in that pcap file.

Finally level 4 (**L4**) transforms are the single function transforms, the ones that look for a specific traffic type or characteristics in a file, such as ARP packets, HTTP hosts, Wireless traffic etc.

My intent is to continue writing/tweaking the Maltego machine(s) so that a lot of these functions are automatic and just a single click rather than running through each transform.

I've also added some new transforms since my last update, here are some of the more "fun" ones:

- GeoIP Map (builds a map you can open in a browser)
- Service Identification (so you can get a quick view of what service is in the pcap based on source/destination ports)
- Null Share and NBT Announcements

Over time I will probably re-write some of the existing ones, or retire them but this is a project I will be working on for a long time as I enjoy it, it teaches me new things every time I write a piece of code and because I hope at least one other person finds it useful.

Oh I'm also nearly ready to release the beta of **Kisploitego**, which are Maltego transforms for wireless (sniffing beacons, deauth attacks, arp cache poisoning etc).

If you have any questions, comments or requests let me know, will be nice to get some comments other than the spam I usually get.

Adam
