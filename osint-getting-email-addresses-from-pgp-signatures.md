---
title: "OSINT: Getting email addresses from PGP Signatures"
date: "2019-06-10"
categories: 
  - "darkweb"
  - "osint"
  - "python"
---

This morning I was reviewing some new Tor onion addresses one of my scripts had collected over the weekend, a lot of the websites I collect are either junk or no longer working.

One of the ones I checked was working, and on the website there was a PGP signature block. That got me thinking if you don’t know someones email address can you determine it from the PGP signature block??

Well the answer is yes, yes you can and with a little of time and some Python (who doesn’t love Python on a Monday morning) you can work out the email address(es) associated from just a PGP signature. The magic comes in determining the KeyId associated with a PGP signature and then using that Key ID querying a PGP key server to find the email address.

The code only currently works on PGP signatures that are the format as the one below (I may update it to work on other formats):

DISCLAIMER: I found this on the “darkweb” so I hold no responsibility/liability for what you do with it.

——-BEGIN PGP SIGNATURE ——-

iQIzBAEBCgAdFiEEekpX72xWccPpVa0cjllHEKAeV0MFAlzthwgACgkQjllHEKAe V0N9pxAApaQWg36TkemWvTu5a4BNuDEVjRQlnYPXg3XGllnIhsMFbiJvUbUJ4JHB 9qe979q1NEjHUaj2Z9u5n7zEs3w6DseZ5/yHAX63ozYfzhTCoix1SZmd+s5bAuPD EvxCWPDe9jZSnNEPdMSsUMaD47YzNhQW8ZZniefBh1VUArZoUN96EaTk3KNsWYza lzDH5gs+dALohQYdQiXp65bOsX0jrs7ouqBbY0kEdr/KymPLtiHsOPzzOgOrkLX8 t0oide+/wowbgV/8efH2ryu9s+hr1imVOIeH//0sjm6l64elNv3gp+81hQwCYtTN NaHP2xDLq/pceFBDksWcPBSrzg4Zm7zJ7Tq0DJ/2HakM0RlpHUVnUoel/SYPWjYY 46Hwom0m5uFE6Lwf8uFaoWnxVYlppt0RxBefIFFX1EBpTCgGgkU6ARInLt0OfHee M5xmgIjv6DXuebfZ0C1I+tMHI/NcnypUt+fKFTQyI0j6GqJcmIWCRquKHb5Z9LXk HP491lO/9irqvTf9HYXWTB6pS7pjV8oZQ1E07CEOKcsfBymYoiJ6cCWTNsGikmNM M7YhxK+ebF78YO13jbKg2hvEtasRtHfrVfB1GLO8XchUkMQx0ib7sktZf6TRAO/F E5UJnleebONjITCiNffP7yys3E5EbaHJkcq/0bbbUs0H0bqHIe0==

——-END PGP SIGNATURE -—-

There is a specific RFC associated with PGP formats, which you can find [HERE](https://tools.ietf.org/html/rfc4880). I’ve not read it to be honest (it’s not that kind of Monday morning). In order to get the Key ID you need to a few steps.

Firstly we need to strip out the start and end of the signature block (the bits with — — ).

\[code lang=python\] regex\_pgp = re.compile( r” — — -BEGIN \[^-\]+ — — -(\[A-Za-z0–9+\\/=\\s\]+) — — -END \[^-\]+ — — -”, re.MULTILINE) matches = regex\_pgp.findall(m)\[0\] \[/code\]

Then we need to decode the signature, which is base64 encoded.

\[code lang=python\] b64 = base64.b64decode(matches) \[/code\]

Convert that output to hex, so you can pull out the values you need.

\[code lang=python\] hx = binascii.hexlify(b64) \[/code\]

Then get the values you need.

\[code lang=python\] keyid = hx.decode()\[48:64\] \[/code\]

Once you have the Key ID you can just make a web call to a PGP key server to search for the key.

\[code lang=python\] server = 'http://keys.gnupg.net/pks/lookup?search=0x{0}&fingerprint=on&op=index'.format(keyid) \[/code\]

Then because well I’m lazy I used regex to find any email address.

\[code lang=python\] regex\_email = re.compile(r’(\[\\w.-\]+@\[\\w.-\]+\\.\\w+)’, re.DOTALL | re.MULTILINE) email = re.findall(regex\_email, resp.text) \[/code\]

The full script can be found [HERE](https://github.com/catalyst256/MyJunk/blob/master/decode-pgpsig.py)

NOTE: I’ve not tested it exhaustively but it works enough for my needs.
