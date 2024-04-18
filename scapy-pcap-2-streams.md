---
title: "Scapy: pcap 2 streams"
date: "2013-10-21"
categories: 
  - "archive"
---

Morning readers, I thought I would start Monday morning with another piece of Scapy/Python coding goodness. This time though for an added treat I've thrown in a bit of **tshark** not because Scapy isn't awesome but for this piece of code tshark works much better.

The code today, takes a pcap file and extracts all the **TCP** and **UDP** streams into a folder of your choice. If you've ever used Wireshark then you know this is a useful feature and it's something that I've put into [sniffmypackets](https://github.com/catalyst256/sniffMyPackets) as it's a good way to break down a pcap file for easier analysis.

The code isn't perfect (when is it ever), at the moment you will get the standard **"Running as user "root" and group "root". This could be dangerous."** error message if like me you run this on Kali. If you are using "normal" Linux you should be fine. I am looking at how to suppress the messages but the Python subprocess module isn't playing nice at the moment.

The code has 2 functions, one for handling TCP streams, the other for UDP streams. For TCP streams we use the tshark **"Fields"** option to run through the pcap file and list of the stream indexes, we save that to a list (if they don't already exist) and then we re-run tshark through the pcap file pulling out each stream at a time using the index and then write it to another pcap file.

For the UDP streams it's a bit more "special", UDP streams don't have an index the same as TCP streams do, so we have to be a bit more creative. For UDP streams we use Scapy to list all the conversations based on **source ip, source port, destination ip, destination port** we then add that to a list, we then create the reverse of that (I called it duplicate) and check the list, if the duplicate exists we delete it from the list.

Why do we do that?? Well because we can't filter on stream, parsing the pcap file will list both sides of the UDP conversation which mean when we output it we end up with twice as many UDP streams as we should have. By creating the **duplicate** we can prevent that from happening (took me a while to figure that out originally).

Once we have created our list of UDP streams we then use tshark to re-run the pcap file but this time with a filter (the **\-R** switch). The filter looks a bit like this:

`tshark -r ' + pcap + ' -R "(ip.addr eq ' + **s_ip** + ' and ip.addr eq ' + **d_ip** + ') and (udp.port eq ' + str(**s_port**) + ' and udp.port eq ' + str(**d_port**) + ')" -w ' + dumpfile`

The parts in bold are from the list we created when we pulled out all the UDP conversations. Each UDP stream is then saved to a separate file in the same directory as the TCP streams.

To run the code (which can be found [HERE](https://github.com/catalyst256/MyJunk)) you need to provide two command line variables. The first is the pcap file, the second is your folder to store the output. The code will create the folder if it doesn't already exist.

`./pcap-streams.py /tmp/test.pcap /tmp/output`

When you run it, it will look something like this:

[![pcap2streams1](http://theitgeekchronicles.files.wordpress.com/2013/10/pcap2streams1.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2013/10/pcap2streams1.png)

If you then browse to your output folder you should see something like this (depending on the number of streams etc.

[![pcap2stream2](http://theitgeekchronicles.files.wordpress.com/2013/10/pcap2stream2.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2013/10/pcap2stream2.png)

So there you go, enjoy.
