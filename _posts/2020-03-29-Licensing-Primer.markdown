---
layout: post
title:  "Licensing Primer"
date:   2020-03-29 11:55:35 -0700
categories: software license cc gpl
---

# ADD LINKS FOR TOPICS SUCH AS BINARY BLOB, RAID, ETC

I love free and open source software. When I was 15 I worked all summer to buy my first laptop, and one of the first things I did was start experimenting with Linux. As anyone with experience from the early 2000's can attest, putting Linux on a laptop at the time was an exercise in masochism. A fresh install ususally left you with a bare terminal prompt, no wifi connectivity, and no power management. If you happened to be able to start a window environment (a term for the GUI), you had no hardware acceleration, which made most graphics-intensive applications take a huge performance hit. Not exactly a 'user friendly' experience. I immediately started asking 'Why? Why is my computer so much less functional with this new software as opposed to the pre-installed OS?' The answer is a two-part problem.

1. _Binary Blobs_
    - Computers are a combination of components made by a wide variety of manufacturers, all of which require specific low-level software to become usable; called *drivers*.
    - Drivers contain a lot of information about the hardware design and capabilities of a given component, which is intellectual property companies generally want to protect. So they make a proprietary 'closed source' driver.
    - In order to protect this information but still allow computer manufacturers and end users access to the hardware capabilities, component makers release a pre-compiled driver that contains "binary blobs"; code that is not human-readable and loaded at a very early stage in the boot process to allow little to no user interaction with it.
    - These blobs provide much of the functionality desired by devices such as GPUs, advanced CPU features, wifi card access, and advanced hard drive controllers such as RAID.