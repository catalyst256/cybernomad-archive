---
title: "Project Watcher - The release"
date: "2014-02-12"
categories: 
  - "archive"
---

So I've had this ready to go (with a few tweaks) for a couple of months now, but for some reason I haven't felt like making it public. Call it laziness, lack of mojo or whatever but I decided this morning just to stop making excuses and let it loose.

[![ProjectWatcherTitle](http://theitgeekchronicles.files.wordpress.com/2014/02/projectwatchertitle.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2014/02/projectwatchertitle.png)

Project Watcher is a Canari Framework transform pack that allows you to perform wireless scanning within Maltego. Making use of aircrack-ng components and Scapy (my favourite thing) it allows dynamic mapping of wireless access points and wireless clients by using a Maltego Machine.

This release has several transforms disabled for the time being, while I finish coding them despite only writing code for over a year I've turned into a bit of a perfectionist which makes releasing my code difficult at times..

The install and usage instructions can be found in the Readme.md file on the Github repo which can be found here:

[Project Watcher](https://github.com/catalyst256/Watcher.git)

The data is stored in a sqlite database and allows for greater flexibility moving forward (oh I used a buzz word) and you can export the data to csv or zip for use elsewhere.

There are a number of things that I will be adding over the next few weeks which include (but not limited to).

- Wigle.net lookups
- Google Mapping
- Shodan searching
- Wireless attacks

In addition to these features I have a **"Phase 2"** planned which is a massive piece of work for me but I'm hoping will create a searchable database of information that can be used to collate and track devices based on their wireless footprint.. but that's still in the planning and I need to learn a few more skill sets to get that off the ground.

This is the kind of graph you can end up with...

[![WatcherExampleGraph](http://theitgeekchronicles.files.wordpress.com/2014/02/watcherexamplegraph.png?w=300)](http://theitgeekchronicles.files.wordpress.com/2014/02/watcherexamplegraph.png)

Enjoy.. and as always let me know if you have any issues..
