---
title: "Python: Kippo 2 Cuckoo"
date: "2013-09-16"
categories: 
  - "archive"
---

So I'm a bit late with this blog post as I wrote the code a couple of weeks ago, but as they say "better late than never".

A couple of weeks ago on Twitter [@stevelord](https://twitter.com/stevelord) raised a question about the existence of some code that would allow you to take the files that evil hackers upload to a Kippo SSH Honeypot and run them through a Cuckoo Sandbox instance. The response was "No" such a thing didn't exist but wouldn't it be great if it did.

Well that's more than enough reason for me to have a go at writing something, the code I put online (via my GitHub repo) is still quite raw, I've not had any feedback to say if it works properly or not but the limited testing I did seemed to indicate that it does. So this is how it works.

On your Kippo honeypot upload the python script and ensure that it has read access to your kippo directories. There are a number of variables in the script that might need changing these are:

`# User defined settings - change these if they are different for your setup sandbox_ip = '127.0.0.1' # Define the IP for the Cuckoo sandbox API sandbox_port = '8090' # Here if you need to change the port that the Cuckoo API runs on kip_dl = '/var/kippo/dl/' # Kippo dl file location - make sure to include trailing / file_history = 'history.txt' # Keeps a history of files submitted to Cuckoo log_file = 'output-log.txt' # Log file for storing a list of submitted files and the task ID from Cuckoo`

Most of them should make sense to you, but you will need to change them to match your environment (give me a shout if you get stuck).

Once that's all in place the script works like this.

1. Creates a new history.txt file if it doesn't exist (in the same directory as the script is located).
2. Creates a catalogue of files in the download directory for kippo.
3. Checks to see if that file already exists in history.txt (so we don't submit files more than once).
4. Submits the files one at a time to your Cuckoo sandbox using the Cuckoo api server (which needs to be running).
5. Updates the history.txt fiie with the new files.
6. Dumps out the task id and status code so you know if it's worked.

And that's it really, I still need to work on the logging side of things, the log\_file variable doesn't work (as in nothing calls it) so that's next on my list to resolve. If you deleted the history.txt file it will case the script to submit everything again so be careful with it.

The script can be found [HERE](https://github.com/catalyst256/MyJunk/blob/master/kippo2cuckoo.py).

I hope it may be of use to you, and as always let me know if it doesn't work (create an issue ticket on GitHub).
