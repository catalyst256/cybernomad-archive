---
title: "Pandora - Maltego Graph Thingy"
date: "2015-10-14"
categories: 
  - "archive"
---

I talk to a lot of different people about Maltego, whether its financial institutions, law enforcement, security professionals or plain old stalkers (only kidding) and the question I usually end up asking them is this;

> What do you want to do with the data once it's in Maltego?

The reporting features in Maltego are good, but sometimes you want something a little bit "different", usually because you want to add the data you collect in Maltego to another tool you may have, or you want to share the information with others (who don't have Maltego) or just because you want to spin you own reports.

A few weeks ago on my OSINT course we talked about classifying open source intelligence against the National Intelligence Model (5x5x5), so I decided to see if I could write a tool that would take a Maltego graph and do just that. In addition (more as a by-product) you can now export Maltego graphs (including link information) to a JSON file.

I would like to thank Nadeem Douba ([@ndouba](https://twitter.com/ndouba)) for the inspiration (and some of the code) which is part of the Canari Framework and originally allowed you to export a Maltego Graph to CSV format.

Pandora is a simple lightweight tool that has two main uses. The first is a simple command interface (_pandora.py_) that will allow you to specify and Maltego Graph and it just spits out a JSON file.

The second usage has the same functionality but via a simple web interface (_webserver.py_), you can export your Maltego graph to JSON and then get a table based view of the entities held within, you can also click on a link that shows all the outgoing and incoming linked entity types.

This is still a BETA at the moment, the JSON stuff works but the web interface has a few quirks to it. Over the next few weeks I will be adding extra stuff like reporting, the ability to send the JSON to ElasticSearch, Splunk (which has a new HTTP listener available) and some other cool stuff.

You can find Pandora [HERE](https://github.com/SneakersInc/Pandora) and some screenshots are below:

**Maltego Graph (example)**

[![MaltegoGraphExample](https://theitgeekchronicles.files.wordpress.com/2015/10/maltegographexample.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2015/10/maltegographexample.png)

**Pandora - Web Interface**

[![PandoraWebInterface](https://theitgeekchronicles.files.wordpress.com/2015/10/pandorawebinterface.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2015/10/pandorawebinterface.png)

**Pandora - Web interface with imported graph**

[![PandoraWebInterfaceWithGRaph](https://theitgeekchronicles.files.wordpress.com/2015/10/pandorawebinterfacewithgraph.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2015/10/pandorawebinterfacewithgraph.png)

**Pandora - Graph Information**

[![PandoraUploadedJSON](https://theitgeekchronicles.files.wordpress.com/2015/10/pandorauploadedjson.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2015/10/pandorauploadedjson.png)

**Pandora - Link Information**

[![LinkInformation](https://theitgeekchronicles.files.wordpress.com/2015/10/linkinformation.png?w=300)](https://theitgeekchronicles.files.wordpress.com/2015/10/linkinformation.png)

As always any questions, issues etc etc please let me know.
