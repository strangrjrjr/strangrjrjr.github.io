---
layout: post
comments: true
title:  "A Series of Tubes, Part 2: ActionCable in Rails"
date:   2020-06-23 13:28:51 -0700
categories: actioncable websockets project
---

In this post we'll dive into implementing WebSocket connections in Rails. If you're interested in a higher level introduction to WebSockets and other communication protocols, check out part one of this series [here](https://strangrjrjr.github.io/actioncable/websockets/project/2020/06/13/A-Series-of-Tubes-Actioncable-Pt1.html).

## Rails Generators

On of my favorite parts of Ruby on Rails is [generators](https://guides.rubyonrails.org/command_line.html#rails-generate). Generators will automatically create files and folders which conform to Rails convention in a single line of code. In fact, `rails new` is itself a generator which will create a functional, if empty, rails app from scratch. After you've run `rails new` and decided on a database schema, you're ready to start using generators. The syntax is pretty straightforward; here's an example from my recent capstone project [Emissary](https://github.com/strangrjrjr/emissary_api): 

`rails g controller User username:string email:string password:string` 

Generators are so powerful because they can be customized to only create the resources you need. Here I'm asking only for the controller (and by extension the model) for my User class. I'll be writing my frontend in React, so no need to clutter my file structure with views for the User. I know from planning my project that I expect to store a username, email, and password for my users and each will be a string. One line and I'm ready to go!

## Channels

Now that we have the basics of generators down, we can dive into ActionCable. ActionCable is a WebSocket implementation that was integrated into Rails 5. ActionCable works by establishing a WebSocket connection with clients, and then clients subscribe to channels to receive content. My app requires 2 connections at the moment, one for new conversations and one for messages within a given conversation. We can fire up our generator by running `rails g channel message && rails g channel conversation` and delve into the structure from there.

In my `app` folder I'll now find a `channels` folder as well as the traditional `models` and `controllers` folders. Much like controllers, channels inherit from a parent class called `ApplicationChannel`. Each of my new channels contains 2 new methods `subscribed` and `unsubscribed`. These manage what happens when a client (henceforth called a consumer) establishes a connection with the channel. I use these methods to manage what data to send to which users. For example, here's my `MessagesChannel` subscribe method:

{% highlight ruby %}
def subscribed
    conversation = Conversation.find(params[:id])
    stream_from "conversation_channel_#{conversation.id}"
end
{% endhighlight %}

This method expects params from the consumer that will include a conversation id. First I search the database for a matching conversation to help filter out bogus parameters, then I establish a _stream_. `stream_from` is a built in ActionCable method that will now broadcast any newly published messages to the consumers subscribed to this conversation. 

`unsubscribed` simply manages what to do when a consumer disconnects from the channel. I chose to utilize another built in method `stop_all_streams` to close the channel on the server side and not broadcast needlessly. 

We need a third method, however, to complete our `MessagesChannel` implementation: `received`

## Receive

This method is where all the work happens. One of my goals for this project was to remove as many RESTful routes as possible and use WebSockets to achieve the same goals. As it turns out, channels are hooked into the models and the database just like controllers are, thanks Rails! Here's my `receive` method:

{% highlight ruby %}
def receive(data)
    leeway = 30
    @user = User.find(JWT.decode(data["user_id"], ENV['JWT_SECRET'], true, {exp_leeway: leeway, algorithm: 'HS256'})[0]["user_id"])
    @conversation = Conversation.find(data["conversation_id"])
 
    if @user
      if @conversation.users.include?(@user)
        @message = Message.create(text: data["text"], user_id: @user.id, conversation_id: @conversation.id)
        ActionCable.server.broadcast("conversation_channel_#{@conversation.id}", @message)
      end
    end
end
{% endhighlight %}

There's a lot going on here, but at its core this should look familiar to a Rails developer. This method takes a JWT as an argument, decrypts the payload, and performs the same authorization, database reads, and commits that `MessagesController` used to handle via HTTP. That payload includes all the parameters we need including: the sending user's id, the conversation id, and the message itself. We use ActionCable to broadcast the newly received message to all users subscribed to that specific conversation. Instant messaging achieved!

This implementation works, but I want to optimize it further in the future by using another Rails component [ActiveJob](https://edgeguides.rubyonrails.org/active_job_basics.html). ActiveJob allow me to implement a queuing system for interacting with the database and allows me to prioritize and schedule actions in the background. I believe ActiveJob will help my application scale and handle increased traffic more gracefully.

### Stay tuned!

Hopefully you've enjoyed my intro to ActionCable. It's a fascinating feature of Rails, and I'm excited to see how else I can leverage WebSocket communication. My next post will delve into React and how to implement client-side WebSocket connections. :wave: