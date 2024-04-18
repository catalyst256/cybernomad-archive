---
title: "sniffmypackets - The Future.."
date: "2014-05-18"
categories: 
  - "archive"
---

So about a year ago I started work on "sniffMyPackets", the Maltego transform set (using Canari Framework) for analysing pcap files. I started it for 3 reasons;

1. I'm obsessed with pcap files (I admit it, I'm an addict)
2. I wanted to start writing Python code
3. It sounded like fun

At the end of last year I hit a wall, 68 transforms in with what I think is a pretty cool "flow" to it and I ran out of ideas, direction, steam, whatever you want to call it. So I haven't really done much with it since.

Then over the last few months, with the Scapy workshop at BSides and a mental kick up the ass I decided that I should give it some love (virtual of cause). The next question was "OK so how do I make it better??", I had an idea, I thought it was awesome that was going to be the next big release for SmP (sniffMyPackets), but as the weeks have gone on I'm starting to wonder if I"m looking at this wrong. What benefit would this give the people that use it??

Let me try and explain... (bear with me)

People write tools to help other people, to enable them to do the same task over and over again with little effort and to ensure the results are predictable (within a small margin of error). So I had written a tool (SmP), within another tool (Maltego), using another tool (Canari Framework) to enable me to do that. To be honest when you write it down like that it seems a little crazy. In order to use my tool you need another 2 in place already!!

One of the things that bugs me at work, is when people say "Sorry can't do that, I need this tool and it costs $$$$", honestly the amount of times I want to kick them in the nuts and say "Open your eyes, we have loads of tools already, just make the most of them". The reality is that in IT (especially Operations), we like off the shelf tools, because we're not developers, we don't code, we just use the same tools as everyone else does.

So why do I tell you this?? Well so my "awesome" idea for SmP was to start integrating pcaps into a database, with a web front end and then rewrite the Maltego transforms to suck out data from the database. I honestly thought that would be cool. Then while researching I discovered there are lots of "tools" out there already. Things like Splunk, LogStash, ElasticSearch, MongoDB (for databases), so why I am retooling something just for the sake of it (other than learning how to do it)??

So from now on I'm going to write tools that let you integrate into existing toolsets, I want you to use my code to get more out of what you already have rather than forcing you to get another toolset to manage and support. You want pcap files in Splunk??? Right I'm going to write you a App for that, you want to use ElasticSearch?? Awesome I'll do that as well. You want pcap files in json? Easy..

The existing Maltego set for SmP will stay (as I have at least 1 user) but I will rewrite them to plug into existing toolset and suck the same data out, that way you don't need more tools, you can just get more of the ones you've already committed to.

Stay tuned...

PS sorry for any spelling/grammar issues, I'm not wearing my glasses..
