---
title: "Project Watcher - MAC Address Lookup"
date: "2014-02-20"
categories: 
  - "archive"
---

Hello,

The release of my wireless Maltego transforms was last week and as promised I've starting adding new features (well ones that I started and didn't finish in time). Today's transforms allows you to look up the MAC address of a wireless client to try and identify the vendor.

We use a sqlite3 database that contains a list of MAC addresses and vendors and within Maltego there is a transform available for **Wireless Clients** called "**Watcher - MAC Address Lookup**". If it finds a match it will create a new "**Vendor**" entity and display that under the Wireless Client entity.

The GitHub repo for the Project Watcher is [HERE](https://github.com/catalyst256/Watcher).

Any questions or queries just give me a shout and if you have a problem with Project Watcher then create an issue on GitHub.

Enjoy.
