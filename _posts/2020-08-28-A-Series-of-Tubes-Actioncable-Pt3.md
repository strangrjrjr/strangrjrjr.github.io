---
layout: post
comments: true
title:  "A Series of Tubes, Part 3: ActionCable in React"
date:   2020-08-28 10:05:12 -0700
categories: actioncable websockets project
---

I'm sure everyone has been waiting with bated breath for the final installment in my series on ActionCable. The moment has arrived! If you've stumbled upon this without reading [part 1](https://strangrjrjr.github.io/actioncable/websockets/project/2020/06/13/A-Series-of-Tubes-Actioncable-Pt1.html) or [part 2](https://strangrjrjr.github.io/actioncable/websockets/project/2020/06/23/A-Series-of-Tubes-Actioncable-Pt2.html), I recommend heading back to those.

## ReactionCable :stuck_out_tongue_winking_eye:

This implementation of WebSockets in React relies upon a properly configured Rails backend. I recommend checking out Part 2 of my series (linked above) to see the Rails server configuration. I'll do my best to describe an architecture-agnostic way of implementing the connections.

First, ensure that you have the `actioncable` npm package installed via `npm install actioncable --save`. This package gives us all the functions we'll need to talk to our backend. 

As a quick aside, I decided to use the [require syntax](https://stackoverflow.com/questions/33248012/how-to-use-react-require-syntax#33251937) to include the `actioncable` package. It functions just like a traditional React import, but is more concise: 

`const actioncable = require("actioncable")`

Throw that in along with your imports and you've got access to actioncable in your component/container!

## Consumers

Next we need to create a _consumer_. The consumer creates a persistent WebSocket connection to our backend route via an [HTTP Upgrade request](https://developer.mozilla.org/en-US/docs/Web/HTTP/Protocol_upgrade_mechanism). Following React best practices, I created this consumer in the `componentDidMount` method:

{% highlight javascript %}
 componentDidMount = () => {
        fetch(`http://localhost:3000/conversations`, {
          headers: {
              Authorization: `Bearer ${localStorage.getItem("token")}`
          }
         })
        .then(res => res.json())
        .then(json => {
        if (json.error) {
          this.setState({error: true})
        } else {
            this.setState({conversations: json,
            })
            const ac = actioncable.createConsumer('ws://localhost:3000/cable')
            ac.subscriptions.create({channel: "ConversationsChannel"}, {
                connected: () => {console.log("connected ConversationsChannel")},
                disconnected: () => {console.log("disconnected ConversationsChannel")},
                received: data => {this.handleReceivedConversation(data)}
            })
            this.conversationChannels = []
            json.forEach(conversation => {
            this.conversationChannels[`${conversation.id}`] = ac.subscriptions.create({
                channel: "MessagesChannel",
                id: conversation.id
            },{
                connected: () => {console.log("connected", conversation.id)},
                disconnected: () => {console.log("disconnected", conversation.id)},
                received: data => {this.handleReceivedMessage(data)}
            })
            } 
            )
            ac.subscriptions.create({channel: "AppearancesChannel"}, {
              connected: () => {console.log("connected AppearancesChannel")},
              disconnected: () => {console.log("disconnected AppearancesChannel")},
              received: data => {this.handleAppearances(data)}
            })
        }})
        }
{% endhighlight %}

That's a *hefty* method :grimacing:, so let's step through it.

1. Once the React component mounts, it makes an HTTP request via `fetch` for all conversations including the current user.
2. Next is some error handling, otherwise it loads those conversations into state.
3. Now we create our consumer via `const ac = actioncable.createConsumer('ws://localhost:3000/cable')`
4. The consumer maintains our connection to the Rails server, and allows us to create **subscriptions**!

## Subscriptions

Subscriptions are the core of WebSocket connections. They allow us to define more granular communication channels and define actions to take when those channels are opened, closed, and receive data.

{% highlight javascript %}
ac.subscriptions.create({channel: "ConversationsChannel"}, {
    connected: () => {console.log("connected ConversationsChannel")},
    disconnected: () => {console.log("disconnected ConversationsChannel")},
    received: data => {this.handleReceivedConversation(data)}
})
{% endhighlight %}

The `subscriptions.create` method takes several hash arguments: `channel`: The Rails channel you'd like to subscribe to, and `connected`, `disconnected`, and `received`. Each of these three are callbacks that govern how the application responds when the subscription is created, destroyed, and gets data. As you can see in the function above this pattern can be reused for multiple channels, allowing for an extensible way to route data to different parts of the application. I use separate subscriptions for _Conversations_, _Messages_ and _Appearances_, each with a unique callback method for handling events and data.

## Send it!

We now have our pathways established, all that remains is to use them! Our receive methods will handle data coming from the server, but we still need to implement a way to send data.

Here is an example of the methods to create a new conversation using proper React syntax of using an event handler and callback function:

{% highlight javascript %}
handleCreateConversation = () => {
        const conversation = {title:this.state.title, topic: this.state.topic, users: this.state.selectedUsers}
        this.onAddConversation(conversation)
       // push history to messageContainer view
         this.props.history.push('/home')
}

onAddConversation = (conversation) => {
    console.log("ONADDCONVERSATION BEING CALLED")
    this.conversationsChannel.send({
        title: conversation.title,
        topic: conversation.topic,
        users: this.state.selectedUsers,
        user_id: localStorage.getItem("token")
    })
}
{% endhighlight %}

Our event handler builds a payload out of the data received from the `newConversationForm` component and passes it to `onAddConversation` which calls our final ActionCable function: `send`! `send` uses our channel created during `componentDidMount` to send the conversation data (along with a JWT for auth) to the backend. Finally our WebSockets loop is complete! We can successfully transmit and receive data to our server in real time.

## Until next time :v:

I hope you've enjoyed this series; WebSockets are a fascinating protocol. As a disclaimer, I'd love to figure out how to implement the actioncable consumer in a Redux store. I think centralizing the creation of that connection will allow for cleaner code and better management of subscriptions. With the store in place, I could start working on additional features leveraging WebSocket connections such as typing indicators and user statuses. Thanks for reading!
