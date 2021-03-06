---
title: rsync &#45;&#45;partial
layout: post
---
Living here in East Africa, I'm learning how to be productive with a less-than-blindingly-fast internet connection. It's fun, really: another externally-imposed constraint that calls forth creativity.

I've solved most of my bandwidth problems by using desktop clients (Mail, NetNewsWire, Actiontastic) instead of web-based ones (Gmail, Bloglines, Tracks). But lately a different sort of hurdle has come downloading large files with intermittent internet access---it's quite painful to lose half a Linux install CD when the connection drops!

Now, <code>rsync</code> has long been my friend, but just today I've found its default behavior is <strong>NOT</strong> to resume downloads midstream after a dropped connection as I'd expected---if the files don't match, you start back at the very beginning. But, thankfully, there *is* a command line option you can use to make it right.

Which brings me to the whole point of this post:

```bash
rsync --partial matthewtodd.org:~/downloads/debian.iso .
```
