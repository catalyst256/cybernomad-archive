---
title: "Gobbler: Eating its way through your pcap files..."
date: "2014-05-25"
categories: 
  - "archive"
---

So a while back I blogged about the future of sniffMyPackets and how I was looking at building components for it that would make better use of existing systems you may be using (not over tooling things). Gobbler is the first step of that journey and is now available for your amusement..

Gobbler can be found [HERE](https://github.com/catalyst256/gobbler):

Essentially you feed Gobbler a pcap file and tell it how you want the data outputted. The current supported outputs are:

1. Splunk (via TCP)
2. Splunk (via UDP)
3. JSON (just prints to screen at the moment).

The tool is based on Scapy (as always) and once the initial file read is done it's actually quite quick (well I think it is).

Moving forward I am going to add dumping into databases (mongodb to start with) and Elastic search. On top of this I'm going to start writing protocol parsers for Scapy that aren't "available". I've found an HTTP one written by @steevebarbeau that is included in Gobbler so you can now get better Scapy/HTTP dumps.

To use with Splunk is easy, first in your Splunk web console create a new TCP or UDP listener (see screenshots below).

[![SplunkDataInputs](http://theitgeekchronicles.files.wordpress.com/2014/05/splunkdatainputs.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2014/05/splunkdatainputs.png)

[![SplunkUDPListener](http://theitgeekchronicles.files.wordpress.com/2014/05/splunkudplistener.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2014/05/splunkudplistener.png)

**NOTE: UDP Listeners are better for any pcap with over 10,000 packets. I've had issues with TCP listeners past that point.**

Once your Splunk Listener is ready, just "tweak" the gobbler.conf file to meet your needs:

\[splunk\] server = 'localhost' port = 10000 protocol = 'udp'

If you created a TCP listener, change the **protocol** to 'tcp' and gobbler will work out the rest for you.

To run gobbler is easy (as it should be), from the gobbler directory run this command:

`./gobbler.py -p [pcapfile] -u [upload type]`

I've included a 1 packet pcap in the repo so you can straight away test using:

`./gobbler.py -p pcaps/test.pcap -u splunk`

or if you want to see the results straight away try the json option:

`./gobbler.py - pcaps/test.pcap -u json`

If you load the packets into Splunk, then they will look something like this:

[![SplunkGobblerPacket](http://theitgeekchronicles.files.wordpress.com/2014/05/splunkgobblerpacket.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2014/05/splunkgobblerpacket.png)

This means if you were to search Splunk for any packet from a Source IP (of your choice) you can just use (test\_index is where I am storing them):

`index="test_index" "ip_src=12.129.199.110"`

This means you can then use the built-in GeoIP lookup to find out the City etc using this Splunk Search:

`index="test_index" | iplocation ip_src`

Which would give you back the additional fields so you can do stuff like this:

[![SplunkIPLocate](http://theitgeekchronicles.files.wordpress.com/2014/05/splunkiplocate.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2014/05/splunkiplocate.png)

The benefit of using Gobbler is that I've done all the hard work on the formatting, I was going to write a Splunk app but the people at Splunk have been ignoring me so...

I've successfully managed to import about 40,000 packets via a UDP listening in just a few minutes, it's not lightning quick but it's a start.

Enjoy..

PS. A "rebranding" of my core projects will be underway soon so stay tuned for more updates.. :)
