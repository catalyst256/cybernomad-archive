---
title: "Project Watcher - The next phase.."
date: "2014-03-18"
categories: 
  - "archive"
---

Hello reader(s), I hope you are well and enjoying the onset of spring (here in the UK that means rain.. and lots of it)..

So a few weeks ago I released "Project Watcher" which were my wireless transform for Maltego. It was a bare bones release intended to get them out there and see what people thought. I'm pleased to say I've received no constructive feedback and as such I'm following the same motto as always.. "Sod you, I'm having fun .."

While I was writing the first transforms one of the things that I found was it there weren't any open source Wardriving databases with a nice HTTP based API, so I thought as the next stage of Project Watcher I would create one..

Today I finished the prototype of a what I hope soon to release to the public. Basically it will allow people to use a simple HTTP GET request to query the Watcher database and see if a wireless access point has been collected and stored. This is a "no frills" solution, there isn't a web page to look at just an API (I'm not going to call it a RESTful API because it's not, well not yet).

This was all new to me as I don't code (other than python) but the API will use MongoDB and a Node.js front end to allow people to query the database. To be honest if it doesn't work I will just turn it off, I'm running this at my own expense so it's more about learning new things but if it takes off that would be awesome.

I've written some python that allows me to take the watcher.db (sqlite) database and import into MongoDB and my next piece of work is to write some code be able to import kismet files into the database via a web page (so you guys can have a go).

Longer term I hope to get a web site up to allow people to search and a couple of other bits that I'm keeping secret... :)

So stay turned for updates the next few weeks..
