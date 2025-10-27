---
title: "Advent of Security: Day 2 - Key Rotation (password reset)"
description: Talking to Rocks, post 4.
date: 2025-12-02
tags: [cybersecurity]
---

On the second cyber-mas day my true love gave to me: a key rotation and a
clean cache with no more cooooooo-kiiiiiiiiiiiiiies

## Key Rotations and for the uninitiated: Password Changes

All you uninitiated cyber-secure individuals will understand the dreadful day
that the little IT notification pops up on your screen or into your inbox
asking you to change your password. **See the addendum**, but this is an
important part of your digital hygeine. A good password makes a secure login,
and a better password is just called a **key**.

A key is normally made by running a [one-way function](https://web.archive.org/web/20251202035053/https://crypto.stanford.edu/pbc/notes/crypto/oneway.html) like
ed25519 (an [elliptical curve](https://web.archive.org/web/20251202035115/https://en.wikipedia.org/wiki/Elliptic_curve))
to create a long string of characters called
a private key and a public key. You can read more about how this works
[here](https://web.archive.org/web/20251202035325/https://csrc.nist.gov/glossary/term/public_key_cryptography) and [here](https://github.com/kelvin-bz/public-key-cryptography.git)

Changing passwords is exactly the same as changing keys: a hassle, but often IT
departments will keep a single key on file for a service for a long time. You
can probably guess how that would be bad if a bad actor stole said key, because
now they can access everything you could normally access with the key. Often
without a username.

So for my technically initiated readers, take this as a reminder to rotate your
keys. It isn't going to take near as long as you think, spend an hour and move
on, honestly.

For the technically uninitiated, take this as a reminder to check if your
passwords or accounts got [pwned](https://web.archive.org/web/20251202035554/https://haveibeenpwned.com/FAQs), which you can check on [this site](https://haveibeenpwned.com)

## Addendum

Recently the [NIST](https://web.archive.org/web/20251202035818/https://pages.nist.gov/800-63-FAQ/#q-b05) published a
change to its password recommendation framework: you don't really need to change
your password like IT says you do, but **MAKE SURE IT IS LONGER THAN 16
CHARACTERS** and change it if there is _any_ evidence it has been compromised.
I suggest using a password manager!

## Links

- My preferred password manager: [Passbolt](https://passbolt.com)
- Government of Canada password recommendations: [GC](https://www.cyber.gc.ca/en/guidance/best-practices-passphrases-and-passwords-itsap30032)
- Check if you've [been pwnd](https://haveibeenpwned.com)
- Identity security [if you got pwnd](https://web.archive.org/web/20251202040313/https://www.cyber.gc.ca/en/guidance/have-you-been-hacked-itsap00015)
