---
title: "Advent of Security: Day 7 - Virtual Machines"
description: Talking to Rocks, post 4.
date: 2025-12-07
tags: [cybersecurity]
---

On the seventh cyber-mas day my true love gave to me: virtual machines,
spam-filtered mail,
a Password Man-a-geeeer,
a clean file-tree,
an antivirus scan, a
key rotation and a
clean cache with no more cooooooo-kiiiiiiiiiiiiiies

## The Malware Ecosystem

When we think of malware, we often think of a nebulous concept of some super
advanced piece of software put onto our system by some bad actor. Unfortunately,
that is rarely the case. Most malware is contained in things people download
off of the internet, and keeping yourself safe is mostly a matter of making
sure the files, packages and programs that you download are safe.

For example, many people will pirate (and power to them) video games, movies,
TV shows and other media. In and of itself, this is a risky activity, but
one simple thing you can do to protect yourself (if you engage in these
activities) is to download media in a virtual machine.

## Virtual Machines

Virtual machines are like a second computer running inside your real computer.
They appear to have all the functions of a normal computer, except that it all
the functionality is simulated by your real (which we call 'host') computer.
A simple way to protect yourself against ransomware is to download your 'risky'
media inside a virtual machine instead of your host machine.

Now, there's a whole sequence of steps you can run on the file once it's
downloaded. A simple workflow to follow:
1. Download your 'risky' file inside your virtual machine
2. Scan the file inside your virtual machine with an antivirus scanner
3. **If the file is safe** transfer it out onto your real machine
4. **If it is not safe** keep it in the virtual machine (VM)

Now, I must disclaim that this is not foolproof. There are ways to bypass the
separation of host and virtual machine, _but_ they are difficult to exploit and
constantly being patched.

There are a few different ways to use virtual machines, but by now most people
have virtualization software on their devices without knowing it:
- **Microsoft Windows** (_shudders_) Hyper-V is included in Windows PRO, but
  you can also use QEMU (if you're a more technical user).
- **\*nix + Apple** Use QEMU or ProxMox if you want a larger
  virtualization infrastructure.

You can also use VMware, but here's my little plug to boycott them since they
are [constantly squeezing the small and medium business
owners](https://www.theregister.com/2025/10/28/cispe_ecco_broadcom/) (Broadcom
owns VMWare).

Another option is VirtualBox, but beware that they [switched their
licensing](https://www.theregister.com/2025/07/30/licensing_change_oracle_virtualbox/)
so don't use it at work!

## Links

- [Hyper-V](https://learn.microsoft.com/en-us/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v?tabs=hyper-v-manager)
- [QEMU](https://www.qemu.org)
- [ProxMox](https://www.proxmox.com/en/)
- [VirtualBox](https://www.virtualbox.org)
- [Example VM Escape Attack](https://security.utoronto.ca/advisories/canssoc-advisory-active-exploitation-vmware-zero-day-vulnerabilities/)
