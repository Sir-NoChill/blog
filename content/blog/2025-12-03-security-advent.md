---
title: "Advent of Security: Day 3 - Antivirus Scan"
description: Talking to Rocks, post 4.
date: 2025-12-02
tags: [cybersecurity]
---

On the third cyber-mas day my true love gave to me: an antivirus scan, a
key rotation and a
clean cache with no more cooooooo-kiiiiiiiiiiiiiies

## Antivirus Scans

Most modern operating systems have some sort of antivirus/virus detection
capability built in. Truth is, if you stay on mostly trusted sites and don't
click the adds you're probably fine, but it is always a good idea to run an
overnight scan every once in a while.

### Windows

Windows defender is all you need. Unless you're dealing with sensitive research
in which case you should listen to IT, they know your compliance requirements.

### Apple

XProtect is all you need. Unless, again, you're dealing with sensitive things
or downloading applications from outside the AppStore (_shudders in cringe_)
(not because you're downloading things from outside the AppStore, but because
apple doesn't let you have free rein over your computer).

I don't know apple, so I can't really offer much advice here. Be safe apple
folks.

### Linux

The problem: hackers love Linux...

The other problem: hackers love Linux!

If you are worried about malware on your Linux box, you should consider using
the following tools:
- [Yara](https://yara.readthedocs.io/en/stable/gettingstarted.html)
- [Maldet](https://github.com/rfxn/linux-malware-detect)
- [ClamAV](https://www.clamav.net)

In Linux, depending on your setup, malware can be much harder to detect. Most
commonly, you will be affected by ransomware (normally through torrents and
other malicious websites). The best place to stop a traditional 'hacker' is
by running a good firewall, closing all possible ports on your local network
and disabling root login.

If you have SSH, just never ever ever ever ever (...) set it up so that you
can log in with a password.

## Links

Unfortunately reddit is my most prominent source for today, and they don't let
archive archive them.
- [On ClamAV](https://www.reddit.com/r/linuxquestions/comments/1o1ymqv/is_clamav_viable_to_use_as_a_simple_scanner_every/)
- [On Linux Security](https://www.reddit.com/r/linuxquestions/comments/1cqlbkz/comment/l3t1eje/)
- You can also install [2FA for your ssh logins](https://web.archive.org/web/20251203052513/https://www.digitalocean.com/community/tutorials/how-to-set-up-multi-factor-authentication-for-ssh-on-ubuntu-18-04)
