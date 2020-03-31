---
layout: post
comments: true
title:  "Licensing Primer"
date:   2020-03-29 11:55:35 -0700
categories: software license cc
---

During a recent coding lab, licensing came up in conversation. As a Flatiron student, we use a lot of starter code provided by Flatiron itself, as well many other tools provided by a variety of companies and communities. One project prompted us to make sure we included a license before submitting, but what license is appropriate?


## Licensing

* Code is considered a creative work, and therefore falls under the same kind of legal provisions governing other forms of art and media.
* All computer code is governed by some kind of software license. Even a [lack of a license](https://en.wikipedia.org/wiki/Public_domain) is considered grounds for determining how a piece of software can be used, shared, and modified.
* The main dividing line with software is between 'closed source' and 'open source' software. Closed source code remains proprietary and hidden from the general public, while open source code is freely available to be viewed and used.
* However, just because a piece of code is open source doesn't mean that it can be freely copied, modified, or integrated into other projects.
* Licenses range from trade secrets (having a copy could be a crime), to public domain (anyone can do anything to or with this work). 
* In between these two extremes, most licenses govern if code can be made proprietary or not.
* Proprietisation means "Can I use this code in something I am going to call my own, and retain rights to?"
* A further distinction is commercialisation: "Can I use this code in something I am going to call my own and make money off of?"

### The Creative Commons Family of Licenses

Flatiron includes the [following license](https://learn.co/content-license) with all its code provided to students. It is largely based on the [Creative Commons](https://creativecommons.org/) family of licenses. Creative Commons provides a versatile and nuanced set of licenses that can be mixed and matched to provide a wide variety of use cases:

1. _CC0_: The work is in the public domain, and free to be used, altered, and shared by anyone without attribution.
2. _BY_: The original author must be attributed in any future uses of the work.
3. _SA_: Any derivative work cannot be licensed in a more restrictive way. SA stand for 'Share-alike'.
4. _NC_: This work may only be used for non-commercial purposes.
5. _ND_: The work can only be used verbatim; no remixing or derivative works are permitted.

### The Flatiron Educational Content License

The core of Flatiron's license is the following Creative Common clauses: BY, NC, and SA. If I use or publish any Flatiron School code, I'll need to properly attribute it as coming from Flatiron. I also cannot use it for any commercial purposes, and furthermore I cannot attempt to license it more restrictively. I am able to share it and alter or build on it, as long as I follow these restrictions.

### Why does licensing matter?

I have always been a tinkerer at heart. I bought my first laptop in the early 2000s and immediately loaded Red Hat 9 onto it from cds that came in the back of a Linux manual. It was a pretty painful learning experience. Lack of driver support usually guaranteed you were dumped into a terminal with no wifi connectivity or GUI. Searching for the answer revealed that these problems were in essence licensing problems. The drivers required to use hardware like GPUs or wifi cards (and many other types of hardware and peripherals) were often closed source commercial code distributed as a [binary blob](https://en.wikipedia.org/wiki/Proprietary_device_driver). This sparked a debate in the Linux community, and open source community at large. Should binary blobs be included in open source projects? What if the blob is provided with documentation? Fundamentally: does it compromise our principles as developers of free and open source software if we include proprietary binary-only code? Different communities have answered this question with everything from a [hard yes](https://en.wikipedia.org/wiki/Linux-libre) to [an eventual no](https://en.wikipedia.org/wiki/Android_(operating_system)#Linux_kernel). The debate touches on topics such as device security, hardware compatibility, product lifetimes, warranty support and the ability of users to repair and upgrade their own devices. Licensing is a philosophical issue more than a legal one, and I believe it should be exercised with care for the product, but also respect of the user.