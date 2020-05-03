---
layout: post
title:  "Understanding MQTT - The Basics"
date:   2020-05-02 18:00:00 -0300
categories: MQTT,Engineering
permalink: "/understanding-mqtt-the-basics"
---
What is MQTT, how does it work and why does it matter? In this post, we'll go over this messaging protocol's basics and some use cases for mobile apps.
<!--more-->

# What is MQTT?
In short, MQTT is a very lightweight publish/subscribe messaging protocol. What does this mean? It's a protocol that enables the exchanging of messages between two or more parties in a network (i.e. the internet!). Messages can be anything - from binary data to simple text and data formats like JSON or Protocol Buffers.

# How does MQTT work?
At a high level, MQTT is actually pretty simple. There are three main actors in play:
- Any number of **publishers**: they are the source of messages being published.
- Any number of **subscribers**: they listen for new messages from the **publishers**.
- One **broker**: the broker coordinates sending and receiving messages. There is no direct communication between subscribers and publishers, they all interact through the broker

The broker is crucial: publishers and subscribers do not know each other. Instead, the broker coordinates receiving and delivering messages according to the topic and QoS (more on that later). 

If you're a mobile or front-end developer, chances are you've worked with REST APIs in the past. You will be used to a specific communication flow: the client makes a request, the server processes it and answers with a response. MQTT is different. With MQTT there is no concept of client and server. Instead, there are the three actors above, and any device can play publisher or subscriber (or both!).

The flow is as simple as:
1. Subscriber connects to the broker (and subscribes to a topic)
1. Publisher sends a message
1. Subscriber receives the message

Two concepts are important to mention if you are just learning about MQTT now:

## Topics
A somewhat lousy analogy would be to compare topics to URLs. A topic is a string value used to filter messages in the broker. Publishers send each message to a specific topic, and subscribers listen to the topics that are useful for them. An example topic to track a device's GPS coordinates could be: `device-1a2b3c4d5e6f/coordinates`

## QoS
Let's not go too deep into this one for now. The important part to keep in mind: some messages are more important than others. For our GPS topic above, it may not be crucial for your application to guarantee that every single coordinate is sent and received. This is why QoS exists.

You have to define a QoS both when sending a message (i.e. how important it is that this message is _send successfully_) and when subscribing (i.e. how important it is that this message is _received successfully_). QoS is a number: 0, 1 or 2. The higher the number, the higher the guarantee, but the higher the overhead. Choose wisely.

# Why does MQTT matter? What are some use cases?
Why not just use HTTP for everything, right? Have you ever had to poll a REST API endpoint constantly to keep asking for updates about something? Or push data repeatedly? HTTP has [significant overhead](https://www.keycdn.com/blog/https-performance-overhead), and was not designed for this type of interaction. You may have seen your mobile app draining a lot of battery due to too many HTTP calls, for example.

MQTT to the rescue! It has [significantly less overhead](http://www.steves-internet-guide.com/mqtt-protocol-messages-overview/) when connecting, even over TLS (encrypted connection). Plus the data package is much smaller - meaning less bandwidth is used and transmission is faster overall.

In addition, MQTT is heavily optimized towards slow/unreliable connections. Quoting [MQTT.org](https://mqtt.org/faq):
> [MQTT is] designed for constrained devices and low-bandwidth, high-latency or unreliable networks. The design principles are to minimise network bandwidth and device resource requirements whilst also attempting to ensure reliability and some degree of assurance of delivery

## Example MQTT use cases (in the mobile development world)
MQTT can be very useful in many cases. The options are limitless, but some examples are:
- High-frequency sensor communication. This can be, for example, pushing raw accelerometer or gyroscope data to a server
- Persistent connections that would be implemented as an HTTP polling mechanism - i.e. status updates
- Push style communication where a central server publishes updates or messages to devices (i.e. push notifications)
- Device-to-device communication (i.e. real-time chat)

# So is MQTT the holy grail?
Great, let's ditch HTTP and use MQTT for everything! Everyone wants fast connections and low overhead, right? Not so fast. There are some important differences - HTTP and MQTT can live in peace together:
- An MQTT message has no concept of _response_ like you would get from an HTTP call. This means messages are one-way communication: you send them, the broker receives them and that's it.
- There is no way for the publisher to know when or if a message was received. You can guarantee delivery using a high QoS, but you can't ask the broker to inform the publisher when it happens. If you need this kind of confirmation, you'd probably be better with HTTP.
- HTTP has many different features out of the box: headers, data types, compression, etc. With MQTT you need to implement all those manually if needed. This is especially important for encryption, which comes almost for free with HTTPS but needs to be configured manually for MQTT.

# That all sounds great! Where do I learn more about MQTT?
Now that you know how MQTT works at a high level, there are many great resources where you can learn more in-depth, including some examples. I recommend the following two series of articles:

- This guide by Steve Cope is a great resource to learn more in-depth about MQTT: [http://www.steves-internet-guide.com/mqtt-basics-course/](http://www.steves-internet-guide.com/mqtt-basics-course/)
- HiveMQ has a great series explaining (almost) everything called MQTT Essentials: [https://www.hivemq.com/blog/mqtt-essentials-part-1-introducing-mqtt/](https://www.hivemq.com/blog/mqtt-essentials-part-1-introducing-mqtt/)
