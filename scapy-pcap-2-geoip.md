---
title: "Scapy: pcap 2 geoip"
date: "2013-10-16"
categories: 
  - "archive"
---

So I've been a bit "relaxed" lately with blog posts, simply because I've not had anything to say or share. To be honest the last couple of months my training has gone a bit all over the place and I've not really focused on much other than some changes to sniffmypackets.

What I have been doing for the last few months though, as part of my sniffmypackets work is writing python code that then goes into a Maltego Transform. Most of these are Scapy based and have been sitting in a private GitHub repo. Rather than let them gather dust I thought I would share them with you here. That means for the next few weeks expect a lot more posts, all python & Scapy related.

Today I'm going to share with you my pcap-geoip python script. It's nice and simple using the MaxMind's geoip database it looks through a pcap file (of your choice) and returns you the Latitude, Longitude and Google Maps URL for all public IP addresses. If it spots a private IP address it gives you a warning and continues on it's way.

First off you need the MaxMinds GeoIP database, you can download this manually if you want put I have a script that will do it for you.

NOTE: Both of these files are available in my GitHub repo [HERE](https://github.com/catalyst256/MyJunk):

**geoipdownload.sh**

`#!/bin/sh mkdir /opt/geoipdb wget http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz -O /tmp/geoipdb.dat.gz gzip -d /tmp/geoipdb.dat.gz mv /tmp/geoipdb.dat /opt/geoipdb/`

Like I said nice and easy.. Feel free to change the download location as you wish but you will need to change the python script to accommodate the change.

Next up is the python script (again sorry if I offend the python kings and queens with my code):

The script only needs one user defined variable to run, which is the location of the pcap file. To run the script (once you've updated the geoip database location) just type:

`./pcap-geoip.py test.pcap`

Within the python file you need to change this to match your geoip database location:

`# Change this to point to your geoip database file gi = pygeoip.GeoIP('/opt/geoipdb/geoipdb.dat')`

**pcap-geoip.py**

`#!/usr/bin/env python`

`import pygeoip, sys import logging logging.getLogger("scapy.runtime").setLevel(logging.ERROR) from scapy.all import *`

`# Add some colouring for printing packets later YELLOW = '33[93m' GREEN = '33[92m' END = '33[0m' RED = '33[91m'`

`# Change this to point to your geoip database file gi = pygeoip.GeoIP('/opt/geoipdb/geoipdb.dat')`

`if len(sys.argv) != 2: print 'Usage is ./pcap-geoip.py pcap-file' print 'Example - ./pcap-geoip.py sample.pcap' sys.exit(1)`

`pkts = rdpcap(sys.argv[1])`

`def geoip_locate(pkts):`

`ip_raw = []  # Exclude "local" IP address ranges  ip_exclusions = ['192.168.', '172.', '10.', '127.']`

`for x in pkts: if x.haslayer(IP): src = x.getlayer(IP).src if src != '0.0.0.0': if src not in ip_raw: ip_raw.append(src)`

`for s in ip_raw:  # Check to see if IP address is "local"  if ip_exclusions[0] in s or ip_exclusions[1] in s or ip_exclusions[2] in s or ip_exclusions[3] in s: print YELLOW + 'Error local Address Found ' + str(s) + END else:  # Lookup the IP addresses and return some values  rec = gi.record_by_addr(s) lng = rec['longitude'] lat = rec['latitude'] google_map_url = 'https://maps.google.co.uk/maps?z=20&q=%s,%s' %(lat, lng) print GREEN + '[*] IP: ' + s + ', Latitude: ' +str(lat)+ ', Longtitude: ' +str(lng) + ', ' + google_map_url + END`

`geoip_locate(pkts)`

And there you have it, enjoy.
