---
title: "Maltego: Meta Search Engines"
date: "2018-06-22"
categories: 
  - "maltego"
---

The other day on the train, I was reading “The Tao of Open Source Intelligence” by Stewart Bertram and there is a section when he talks about “Metasearch Engines”. Basically when you use search engines like Google or Bing you are searching their database of results. Metasearch engines typically aggregate the results from multiple different search engines which is useful when performing OSINT related searches or just if you want to save time.

A while back I came across [searx](https://asciimoo.github.io/searx/index.html):

> Searx is a free internet metasearch engine which aggregates results from more than 70 search services. Users are neither tracked nor profiled. Additionally, searx can be used over Tor for online anonymity.

Now I like searx for a number of reasons;

- It’s written in Python (flask).
- You can run it in a Docker container.
- You can create your own (or add) search engines.
- You can search for images, files, bittorents etc.
- It has the ability to output in JSON, CSV and RSS

The part of searx that really interests me is the ability to add your own search engines. I’ve yet to try this but in theory not only can you add “normal” search engines but you should also be able to add your own internal data sources. For example if you have an Elastic search index you could write a simple API to query it and then add it as a search engine within searx.

You could in theory (I am going to test this) remove all the normal search engines and use searx as a search engine over all your internal data sources (some assembly required).

Now what’s any of this got to do with Maltego? Well Maltego (out of the box) has some transforms for passing entities through search engines, however sometimes you just want to “Google” something and see what you get back (well that might just be me).

The idea was to run a docker instance of searx, and then create a local Maltego transform to run a query and return the first 10 pages of results.

Lets break down the steps:

1. Build a docker image for searx.
2. Write a local transform to query searx and return the results.

Before I get into the good stuff, let me quickly explain local transforms (in case you don’t know). Local transforms work the same as remote transforms except that they are run locally on your machine. Local transforms have some benefits over remote transforms but also some downsides as well (not an exhaustive list).

**Benefits:**

- You get all the benefits of the processing power of your local machine.
- You can run transforms that query local files on your machine, for example if you want to have a transform to parse a pcap file you can (more difficult with remote transforms).
- It’s much quicker to develop/make changes to local transforms and you don’t have any other infrastructure requirements.

**Downsides:**

- Its more difficult to share local transforms with others (sharing is caring).
- You have to worry about library dependencies and all that stuff.

Ok enough of that stuff, lets get onto the interesting stuff.

First off we need to get a Docker image for searx, luckily this is really easy as the author’s documentation is really good. The instructions for this can be found [HERE](https://asciimoo.github.io/searx/dev/install/installation.html#id12).

All being well you should have a running instance of searx which should look like the image below:

[![](https://theitgeekchronicles.files.wordpress.com/2020/06/image1.png?w=300)](http://cybernomad.online/2020/06/04/maltego-meta-search-engines/image1/)

Have a play with searx, it’s well worth the time and has lots of cool features.

The local Maltego transform (it’s written in Python 3.x ) is pretty simple, pass it your query (based on a Maltego Phrase entity) and it will return the results as URL entities.

Paterva provide a Python library for local transforms, and similar to the remote TRX library it was written in Python 2.x so I tweaked it to work with Python 3.x.

Here is the code for the transform (Github repo linked at the end).

\[code lang="python"\] #!/usr/bin/env python # -\*- coding: utf-8 -\*-

import requests import sys import json from settings import BaseConfig as settings from MaltegoTransform import \*

def metasearch(query): m = MaltegoTransform() for page in range(1, settings.MAX\_PAGES): url = '{0}{1}&format=json&pageno={2}'.format( settings.SEARX, query, page) response = requests.post(url).json() for r in response\['results'\]: ent = m.addEntity('maltego.URL', r\['url'\]) ent.addAdditionalFields('url', 'URL', True, r\['url'\]) if r.get('title'): ent.addAdditionalFields( 'title', 'Title', True, r\['title'\]) if r.get('content'): ent.addAdditionalFields( 'content', 'Content', True, r\['content'\]) m.returnOutput()

if \_\_name\_\_ == '\_\_main\_\_': metasearch(sys.argv\[1\]) \[/code\]

Now the transform maps across the URL, the title of the page (if it exists) and the content (which is a snippet of text, similar to Google results) as an additional field.

Below is a screenshot of what it looks like in Maltego.

[![](https://theitgeekchronicles.files.wordpress.com/2020/06/image2.png?w=285)](http://cybernomad.online/2020/06/04/maltego-meta-search-engines/image2/)

To get the transform working, clone the Github repo (link below) and then you will just need to add the new transform, if you’ve never done it before Maltego provide instructions [HERE](https://docs.maltego.com/support/home).

One of the things I love about Maltego is it doesn’t need to be complicated, creating transforms is easy, and then using them is just as simple. The ability to define what the tool can do for you, makes it powerful and versatile.

The Github repo is [HERE](https://github.com/catalyst256/maltego-metasearch)
