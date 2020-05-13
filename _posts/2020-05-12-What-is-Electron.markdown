---
layout: post
comments: true
title:  "What is Electron? or: Am I an app developer?"
date:   2020-05-12 19:35:12 -0700
categories: electron framework development
---

Over the past few years I've heard more and more about [Electron](https://www.electronjs.org/), a framework that allows the creation of desktop apps using nothing more than JavaScript, HTML, and CSS. It's become [incredibly popular](https://www.theverge.com/circuitbreaker/2018/5/16/17361696/chrome-os-electron-desktop-applications-apple-microsoft-google), powering the likes of VS Code, Slack, Microsoft Teams, and many other applications. 

## What is Electron?

Well, essentially it's [Chromium](https://www.chromium.org/Home): Google's debranded, open-source base for Chrome, and [Node.js](https://nodejs.org/en/). Yup, all Electron apps are actually web apps. Electron spins up a browser instance and acts as both the client and server, allowing the developer to build front end components in HTML, CSS, and JS. It also includes the ability to write a backend in JavaScript as well. By using Node.js and its [npm](https://www.npmjs.com/) package manager, that JavaScript now has hooks into virtually all parts of the underlying operating system, including the ability to do things like: create and delete files on the hard drive, run terminal sessions, push notifications to the desktop, access location data and webcams without asking for user permission, control the mouse and keyboard, daemons that start at boot and persist throughout the entire user session, and much, much more. All of this functionality works across all major operating systems from one codebase, which is one of the main reasons for its popularity. Electron apps can also be uploaded to the Windows and macOS app stores, putting them side by side with native applications.

## Should I use Electron?

This issue is a thorny one, with [strong](https://medium.com/commitlog/electron-is-cancer-b066108e6c32) [opinions](https://drewdevault.com/2016/11/24/Electron-considered-harmful.html) on [both sides](https://github.com/sindresorhus/awesome-electron). Electron lowers the barriers to entry for standalone application development. It allows for cross-platform development from a single codebase. It's already been adopted by large companies listed above. All of these properties are huge boons, however they're not without costs.

Remember, Electron is a Chromium browser + Node.js. Writing a [simple 'hello world' app](https://github.com/electron/electron-quick-start) requires ~200Mb of disk space, 4 processes, and ~80Mb of RAM. Those numbers continue to grow as the apps do. We've probably all had our machines slow to a crawl and blame that damn web browser with 400 tabs open; well now your text editor is a web browser, your chat client is a web browser, your terminal is a web browser, and on it goes. [Electron is hugely inefficient](https://medium.com/commitlog/why-i-still-use-vim-67afd76b4db6) when compared to native apps, and that should bother us if we want to be called software _engineers_. We should use the right tool for the job, even if it requires learning how to use new tools. 

Another key point about Electron: Electron apps are _not_ native apps. All major operating systems include tools that allow applications to seamlessly integrate into the look and feel of the OS. Electron skips this integration and renders all its own content, leaving apps built on the framework looking out of place and ignoring OS conventions.

As a new developer I see the appeal. It would be very satisfying to use the skills I'm learning in web development and start banging out standalone apps. However it feels like I've learned to use a hammer, and now every problem looks like a nail. Software development is so vast and dynamic that there simply can't be a 'one size fits all' solution. I worry that major companies adopting Electron as their go to framework will lead to stances like: 'Web developers can do everything, so why hire anyone else?' It also places even more influence in the hands of Google, a company not above [leveraging open source projects](https://strangrjrjr.github.io/me/myself/i/2020/04/13/What-is-my-Why.html) to suit their own business goals. Personally, I don't want there to be one web browser, and I also don't want there to be one app framework. 