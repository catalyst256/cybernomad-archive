---
title: "Netscalers: Making sense of the cookie - part 2"
date: "2012-01-06"
categories: 
  - "archive"
---

[![](images/robot-trans-mini.jpg "newheadshot")](http://theitgeekchronicles.files.wordpress.com/2012/02/robot-trans-mini.jpg)At the beginning of the week I wrote [here](http://itgeekchronicles.co.uk/2012/01/03/netscaler-making-sense-of-the-cookie-part-1/) about the Cookie's that the Netscaler uses for persistence. In that post I explained how I discovered that the Cookie name was encrypted using a simple substitution cipher. The cookie value itself was encrypted to contain the Service IP (the IP of the server that your session sticks to) and the Service Port.

I assumed that this part of the cookie was encrypted using a "real" encryption method such as [SHA-256](http://en.wikipedia.org/wiki/SHA-2) or some other similar cipher. I spent the next couple of days looking online to see if I could match the cookie length and output (it's all Hex) to a cipher. In the end I gave up, not because it was too difficult but because I thought of a more cunning plan.. :)

This is an example Netscaler cookie (and by example I mean from a website on the internet);

NSC\_wtsw-bmufsjbo-qvcmjd-iuuq=ffffffffaf18363b45525d5f4f58455e445a4a423660

My previous post dealt with how the "encrypted" cookie name was formed (that's the bit up to the '='), this post is about the 8 characters after the ffffffff (everything else after that apart from the last 4 characters seems to be padding).

This is what I knew about the encrypted values:

1. The cookie started with ffffffff which I believed was not required to identify the Service IP.
2. The output was Hex, so I assumed that there must be some way to reverse engineer the encryption back to the real IP.
3. The encrypted value for each octet of the IP address was not encrypted using the same method (I knew that because when looking at cookie value I could see the same IP octet encrypted to different values in the cookie).
4. The encrypted values were consistent across different Netscalers (ruling out the encryption being based on appliance specific details i.e. hostname or MAC address).

In order to decrypt the Service IP out of the cookie I could decided that using a VPX (Virtual Netscaler) I could generate a cookie value for each of the 255 IP address in each octet, armed with the power of Excel and Notepad I generated the necessary Netscaler config to create my samples and then using this command on the Netscaler;

`show lb vserver [vserver name] | more`

This allowed me to see each server and the matching Netscaler cookie value. I started entering these into Excel with the "real" IP value. I had worked through about 60 of the last octet (starting at x.x.x.0) when I realised that I was seeing a pattern. To work out the pattern I took a wild guess (they are the best sometimes) and tried this in Excel;

\=HEX2DEC(CELL)-Real Value

This was the breakthrough I was looking for.. and here's why

On the last octet of the IP address the Hex value 11 was really 0 if you the formula above you get the result "17", use this formula for the next 16 real values (remember I have collected 60 already from earlier) and you see the following pattern:

Real Value    Difference 0                17 1                15 2                17 3                15 4                17 5                15 6                17 7                15 8                17 9                15 10               17 11               15 12               17 13               15 14               17 15               15

Carry on for another 16 and you find this:

Real Value      Difference 16                -15 17                -17 18                -15 19                -17 20                -15 21                -17 22                -15 23                -17 24                -15 25                -17 26                -15 27                -17 28                -15 29                -17 30                -15 31                -17

The next 16 after this repeated first example, in fact all of the decryption for each octet required a repeating pattern, I just needed to find the key. Before rushing ahead I used the 2 patterns above to fill the remaining last octet of 255 addresses but I swapped the formula to create the Netscaler Hex value (and save myself sometime);

\=DEC2HEX(Difference+Real Value)

I then double checked this was correct by looking at my other generated cookie values and checking some from another 2 Netscalers that use this method in "live". I was one happy geek, I then needed to do the same pattern matching for the other 3 octets, but because I knew I was looking for a pattern I only needed to generate a smaller sample set to work with.

Whereas the first pattern I discovered was based on chunks of 16 the others weren't, the first octet is using the numbers 1 & 3 in chunks of 4 (and the negative values for these as well), the second octet is just based on 8's in chunks of 8(+8 and -8), and the third was totally random (not the pattern, more the logic behind it) and work on 2,6,10,14,18,22,26 & 30 in chunks of 16 again(and then the negative versions).

Rather than boring you with pages of information I've produced a PDF with it all in [here](http://theitgeekchronicles.files.wordpress.com/2012/01/netscalercookieinformation.pdf).

So I've tested this as much as I can, and it works, the cookies I've looked at (where I know the Service IP) matches against this decryption sheet and again that is over 4 different Netscalers, running different appliances, IP addresses and versions of firmware.

Once I learn how to write in some sort of programming language I am hoping to write this into an application, where you can input the cookie value and it will provide you the decrypted values, I can think of a couple of uses outside of Netscaler administration and I'm sure any Pen Testers/Ethical Hackers reading this can probably think of a few more.. :)

So to recap, I now know how to decrypt the Load Balancer name from the Cookie name and the Server IP from the Cookie value, the remaining part is the Service Port but I'm not too worried about that (at the moment) as I know that it if a Netscaler cookie ends 3660 then it's port 80.

Let me know if you have any questions or feel that my maths is wrong somewhere along the line..

Happy cookie decrypting.

The Geek
