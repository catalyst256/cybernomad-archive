---
title: "OSINT: Etag you're it..."
date: "2018-10-25"
categories: 
  - "osint"
---

Investigators, malware analysts and a whole host of other people spend time mapping out infrastructure that may or may not belong to criminal entities. Some of the time this infrastructure is hidden behind services that are designed to provide anonymity (think Tor and Cloudflare) which makes any kind of attribution difficult.

Now I’m a data magpie, I have a tendency to collect data and work out later what I’m going to do with it. A while back someone suggested ETag (HTTP Header) as something worth collecting, so I thought I would have a look at potential uses.

ETag’s are part of the HTTP response header and is used for web cache validation. ETag’s are an opaque identifier assigned by a web server to a specific version of a resource found at a URL. The method for how ETag’s are generated isn’t specified in the HTTP specification but generated ETag’s are supposed to use a “collision resistant hash function” (source: [https://en.wikipedia.org/wiki/HTTP\_ETag).](https://en.wikipedia.org/wiki/HTTP_ETag).

I wanted to see if you could use ETag’s as a way to fingerprint web servers, however ETag’s are optional so not all web servers will return an ETag in the HTTP response header. Let’s look at an example, I have an image hosted on an Amazon S3 bucket, this S3 bucket is also fronted by Cloudflare (mostly for caching and performance). The HTTP headers for both requests are below;

**Direct request:**

\[code lang=text\] HTTP/1.1 200 OK

Accept-Ranges: bytes Content-Length: 1150 Content-Type: image/x-icon Date: Thu, 25 Oct 2018 09:06:25 GMT \*ETag: “e910c46eac10d2dc5ecf144e32b688d6”\* Last-Modified: Mon, 06 Aug 2018 12:18:30 GMT Server: AmazonS3 x-amz-id-2: LxghTaCpRKQhaP69Qm942BrdyhN87m+SIJTh1xz403c5nVEGj4Y7fsPtNrZ2uedLPRL3zCIeHas= x-amz-request-id: 5116840933538C62 \[/code\]

**Through Cloudflare:**

\[code lang=text\] HTTP/1.1 200 OK

CF-Cache-Status: REVALIDATED CF-RAY: 46f3876dbb4b135f-LHR Cache-Control: public, max-age=14400 Connection: keep-alive Content-Encoding: gzip Content-Type: image/x-icon Date: Thu, 25 Oct 2018 09:06:46 GMT \*ETag: W/”e910c46eac10d2dc5ecf144e32b688d6" \*Expect-CT: max-age=604800, report-uri=”https://report-uri.cloudflare.com/cdn-cgi/beacon/expect-ct" Expires: Thu, 25 Oct 2018 13:06:46 GMT Last-Modified: Mon, 06 Aug 2018 12:18:30 GMT Server: cloudflare Set-Cookie: \_\_cfduid=d5bd0309883f2470956c30bdbca9c99751540458406; expires=Fri, 25-Oct-19 09:06:46 GMT; path=/; domain=.sneakersinc.net; HttpOnly Transfer-Encoding: chunked Vary: Accept-Encoding x-amz-id-2: vJsCSPCMJFdBmxdPemrtoX07SCKHliZicOpUYM32Do9TPnCFFLiZxsNXeNo3SK/lv/90sqDBPa0= x-amz-request-id: CDC9A8EF960ABE44 \[/code\]

From this example you can see that the ETag’s are the same, with the exception that the Cloudflare returned ETag has an additional “W/” in the front. According to the Wikipedia article a “W/” at the start of an ETag means it’s a “weak ETag validator”, whereas the direct ETag (not through Cloudflare) had a “strong ETag validator”.

A search on Shodan shows over **23 million** results where ETag’s are shown in the results, this gives us a big data set to compare results against. Let’s look at another example, if we search Shodan for this ETag “_122f-537eaccb76800_” we find **263** results, which shows that ETag’s aren’t necessarily unique. However, if you look at the results the web page titles are the same “Test Page for the Apache HTTP Server on Fedora”. So, it seems that the ETag does relate to the content of the web page, which is in line with the information from Wikipedia.

So, in some instances the ETag is unique, in others it might not be, but it does give you another pivot point to work from.

Let’s have another look at an example, the ETag “_dd8094–2c-3e9564c23b600_” provides **16,042** results. That’s a lot but if you look at the results (not all of them obviously), you will notice that they are all the same organisation “Team Cymru”. So, while the ETag in this example doesn’t provide a unique match it does provide a unique organisation, which again gives you an additional pivot point.

I’m going to continue looking at ways to collate ETag’s to web servers, if nothing else it’s an interesting way to match content being hosted on a web server. Have a look at this ETag “_574d9ce4–264_”.
