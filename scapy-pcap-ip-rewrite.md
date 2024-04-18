---
title: "Scapy - pcap IP rewrite"
date: "2013-08-27"
categories: 
  - "archive"
---

Hello reader(s), this is just a quick post to share some new code I wrote tonight, you might find it useful or you might not.

So I've been trying to think of some new transforms to write for sniffMyPackets and thought it would be cool to take a TCP stream and rewrite the source and destination IP addresses and then recalculate the IP/TCP checksums so you can "replay" the packets if you so desire.

The code is as always written in python and uses Scapy to work the magic, the link to the code is at the bottom of the page, and is stored in another one of my GitHub repo's specially created to share all the "junk" code I write and want to share with you guys..

The usage is really simple, tell the script you source pcap file (the one that you want to rewrite), give it the new source and destination IP addresses and then finally the location you want to store the output pcap file. For example:

`./pcap-rewrite.py input.pcap 192.168.1.1 192.168.1.254 /tmp/output.pcap`

Now just to make clear this is to work on individual TCP streams (might even work on UDP thinking about it), so 1 source IP, 1 destination IP. NOT pcaps files with lots of different IP's.

When you run the script with the variables defined above (did I just use variables in a sentence..too much coding). You should get something like this:

[![terminalwindow](http://theitgeekchronicles.files.wordpress.com/2013/08/terminalwindow.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2013/08/terminalwindow.png)

What you should get inside of your pcap file is something that looks like this (well not like this as it's my pcap file):

[![wireshark-new](http://theitgeekchronicles.files.wordpress.com/2013/08/wireshark-new.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2013/08/wireshark-new.png)

Which use to look like this:

[![wireshark-old](http://theitgeekchronicles.files.wordpress.com/2013/08/wireshark-old.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2013/08/wireshark-old.png)

Notice all the green in the new rewritten pcap file (the first wireshark image for those not paying attention). That's because both the IP and TCP checksum have been deleted and then Scapy recalculate it when the IP addresses are changed (thanks to Judy Novak for that bit of useful information).

So without making you read any more here's the code: [DOWNLOAD](https://github.com/catalyst256/MyJunk/blob/master/pcap-rewrite.py)
