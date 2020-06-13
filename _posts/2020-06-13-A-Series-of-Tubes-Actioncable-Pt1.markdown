---
layout: post
comments: true
title:  "A Series of Tubes, Part 1: Communication Protocols"
date:   2020-05-12 19:35:12 -0700
categories: actioncable websockets project
---

I really enjoy instant messaging; it's one of my favorite things about the internet. Like many others dipping their toes into the world of dial-up in the early 2000s, my first experience with it was [AIM](https://en.wikipedia.org/wiki/AIM_(software)). Unlike email or forums, the best part of AIM was the _Instant_ messaging. So much high school drama played out over chat. As luck would have it, I was assigned a [group](https://github.com/strangrjrjr/flatchat_backend) [project](https://github.com/strangrjrjr/flatchat_frontend) to build a chat application as part of my module 4 curriculum at Flatiron.

## Communication methods

As I've learned how to build web applications, I've come to really enjoy setting up communication between machines. At first I used simple API calls to gather data to seed databases, then I learned how to build my own API to communicate with my own front-end clients via HTTP requests. As I broke into JavaScript I began to utilize AJAX requests, before finally working with WebSockets to build a full-duplex chat application.

### Email: One-way communication

    Email works much like its material counterpart...mail. The message is crafted with an address of where it should go, and it's launched off into the ether. Most of the time it arrives, barring the errant typo, but you'll never know unless the recipient decides to send you a response. (Quite a subject to meditate on as I begin my own job hunt.) 

### Half-duplex: the 'walkie-talkie' method

    Half duplex communication is best embodied by a walkie-talkie example. Two clients establish a bi-directional line of communication, meaning both can send _and_ receive data. However, only one can transmit at a time. If multiple clients attempt to transmit at the same time collisions occur, resulting in lost messages. In order to avoid such collisions, clunky methods can be employed such as ending all transmissions with 'over'.

### HTTP: Request-response

    HTTP works via a request-response cycle. Traditionally, clients make requests to servers, and then wait for responses from the servers for a fixed interval of time. It is purely _synchronous_, meaning only one machine sends data over the wire at a time, with the client and server switching roles between transmitter and receiver until the transaction completes with a [status code](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes). Synchronous HTTP operations are _blocking_, meaning that while either machine waits for its next response, no other processes continue until the time window expires. These kinds of connections also rely on page reloads in order to render new content. The early internet was dominated by paginated sites to manage large amounts of content, limited bandwidth, and the constraints of synchronous communication.

### AJAX: Asynchronous communication via HTTP

    AJAX improves upon regular HTTP requests by shifting them to _non-blocking_ operations. Instead of causing an entire application to hang waiting for a response, the machine can offload HTTP requests to background processes. Now web pages and applications can continuously present content to the user, instead of showing them the dreaded 'white page' while waiting for content from servers. The principle method (or function, as it were) that I've used to make AJAX requests is JavaScript's `fetch()`, coupled with `.then()`. `Fetch` initiates traditional HTTP requests, but doesn't require the application to wait for a response. Chaining `.then` methods on to the fetch allows for additional processing and rendering of data once the request is completed. 

### WebSockets: Full-duplex communication

    WebSockets are a completely different protocol than HTTP. Despite this fact, WebSocket connections are established via HTTP requests, but they utilize a newer capability of the HTTP standard: the upgrade request. WebSockets work using their own URIs, so instead of hitting `http://myawesomesite.com/chat`, connections are established via hitting `ws://myawesomesite.com/chat`. Once the channel is established, both the client and server can send and receive simultaneously. 


## To Be Continued...

I hope you've enjoyed my brief introduction to communication protocols. This article is the beginning of a series I'm writing about my capstone project, [Emissary](https://emissary.netlify.app/). Emissary is a real-time chat application utilizing a [Ruby on Rails backend](https://github.com/strangrjrjr/emissary_api) and a [React frontend](https://github.com/strangrjrjr/emissary). It is a continuation of the group project mentioned in the introduction, with an eye towards expanding functionality to mimic a Slack- or AIM-style application. I am passionate about privacy and security, so I am using my hosted instance as a place to experiment with securing web applications for production environments. Stay tuned for my next post, where I'll dive into implementing WebSocket communication in Rails and React!