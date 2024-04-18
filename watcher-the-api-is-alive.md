---
title: "Watcher: The API is alive!!"
date: "2014-03-30"
categories: 
  - "archive"
---

Hello readers,

So I've just finished pushing my Watcher Project API "live" for you guys to poke around and see what you think. Before I give you the URL here are my standard disclaimers:

1. The API is very basic at the moment and only allows for query by SSID (Access Points).
2. There are currently only 1255 records in the database, all of which are from around my city.
3. The API (and I use the term loosely) is running in the cloud in the smallest possible instance possible so it might be slow, prone to crashing etc. etc.
4. I'm funding this little project so when it gets too expensive to run I will probably have to turn it off.
5. The data isn't backed up but I have the raw files so..

To be honest even now I still get a sick feeling in my stomach when pushing stuff live, call it fear of negative feedback but luckily I tend not to get any feedback so yah me.. :)

All of the access points have been imported from Kismet .netxml files and I've been using Kismet running on an old Nokia N900 pwnphone which works well and has the benefit of having GPS. So if you have any Kismet files you want to share, let me know..

You can access the API over your browser or just by requesting a normal GET request over HTTP to the server. You will get in a return a JSON response similar to this one below.

{ "\_id": "533819e88f86f5769b3f5699", "enc": "None ", "longitude": "-0.243964", "ssid": "BTOpenzone", "bssid": "28:16:2E:3D:F1:D4", "latitude": "52.576906", "type": "infrastructure", "channel": "1", "datetime": "Thu Mar 20 20:30:50 2014" }

Over the next few weeks I will be adding some extra functionality, things like:

1. SSL Certificate
2. More search functions
3. More access points
4. Web interface for searching
5. Some sort of backup (mongodump to S3 bucket or something)
6. Stop it being case sensitive on queries
7. File upload and import for users
8. Some kind of token auth

Most of this is a learning experience for me rather than anything else, I will also add some transform into my Watcher Maltego pack to query the API so it that all ties in nicely.

So here is the URL (see if anyone spots the port number choice).

http://api.watcherproject.net:8021/network/

You then add your SSID to the end, for example.

http://api.watcherproject.net:8021/network/BTWiFi

Here are some SSID's in the database to get you testing.

BTOpenzone McDonald's Free WiFi BTWiFi

Please, please, please let me know what you think and if it gets better/bigger its useful.
