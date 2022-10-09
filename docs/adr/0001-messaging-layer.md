# 1 - Messaging Layer

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 09/10/2022, 01:23:11 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

[n-vyro.io](https://n-vyro.io) requires an event-based communication layer.
This layer needs to allow each device to communicate with each other, along
with our internal systems, and UIs. This will work as our messaging bus, and
allow devices to listen in for new messages and do something with them.

The core purpose of this layer is to provide UIs, users, and services, with a
mechanism for retrieving messages as fast as possible. It is not to be used for
historical data, as this will never been accessible via this layer. So it is
ideal for our UIs, which require real-time data, or our daemons which listen
out for messages and do discreet pieces of work on it.

We don't care whether a message gets to a destination, only that we're able to
send messages, and that we can make our devices listen out for them. This is
will be important for our UIs, and services, as it will be the core mechanism
for retrieving data in real-time.

It will be important that this layer is super fast, as our systems are time
depends on these messages being received within milliseconds. It is also going
to be vital that we're able to be further secure this layer. An added bonus
would be that this layer is light weight, and doesn't require a lot of
knowledge to work with.

Eventually users will be able to use their own messaging service, so we will
need to consider what is common for users to leverage. We also want to make
sure that we can support this functionality from within NVyroHub.

## Decision Drivers <!-- optional -->

* Commonly used communication layer
* Easy to secure
* Able to implement regardless of tech stack (JS, Ruby, C/C++)
* Can install on Linux

## Considered Options

* Websockets
* MQTT
* Kafka
* RESTful API

## Decision Outcome

Chosen options: "MQTT" and "Websockets", because MQTT is ubiquitous within the
IoT ecosystem, and Websockets works well with UIs.

We will make use of `mosquitto` as it is really easy to setup, and it will
allow us to easily setup an MQTT server, with basic authentication. It also
includes a Websocket port, so we will enable Websockets, for our services, to
listen out for incoming messages. This will mean that we can use two options,
Websockets for real-time UI updates, and MQTT for listening and sending device
messages (backend and firmware).

Both "Websockets" and "MQTT" send their payload as JSON as a default. So all of
our stack will be able to easily handle this data.

Once we've refined our microcontroller firmware we will introduce SSL/TLS, and
then we can upgrade our NVyroHub instance with the appropriate security
improvements.

Paho is a very common MQTT library, so much so that the API has been
implemented in all of the languages we use to build n-vyro.io devices. This is
yet another good reason to pick "MQTT". As Paho, allows us to connect via
"Websockets" we can handle our incoming messages as if it was a connection to
MQTT directly.

"Kafka" is quite similar to MQTT, but it requires a hell of a lot more setup,
and it can introduce a number of unexpected behaviours, which we'd have to
cater for. It's typically used in more enterprise solutions, and even there
we've found that it can become quite cumbersome.

A "RESTful API" also doesn't fit the bill, it would take too long to retrieve
the data. We also want to reserve APIs for retrieving historical data, which
would establish a nice boundary between the how we retrieve real-time and
historical data. As an API doesn't provide us with a way of easily listening in
on any incoming messages. We'd also have to handle any failed API calls, and
add API polling for all devices messages, both of which aren't ideal, and can
be buggy.

### Positive Consequences <!-- optional -->

* Ubiquitous within IoT
* Client library support in all used languages
* Installs on Linux
* Fire and forget
* Informal
* Easy to integrate into our provisioning tools

### Negative Consequences <!-- optional -->

* Initially insecure (basic authentication)
* Requires some Linux knowledge

## Pros and Cons of the Options <!-- optional -->

### Websockets

https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API

* Bad, unknown how to setup in C/C++
* Good, fire and forget
* Good, easy to learn
* Good, easy to use
* Good, easy to setup via UI

### MQTT

https://www.eclipse.org/paho/

* Good, easy to setup
* Good, Ubiquitous with IoT
* Good, Linux knowledge
* Good, others know
* Good, Easy to learn
* Bad, Linux knowledge
* Bad, Message payload limit (256MB)

### Kafka

https://kafka.apache.org/

* Bad, Complex
* Bad, More than we need
* Good, Linux based
* Good, Fast
* Good, other Rubyist know

### Restful API

* Bad, Requires polling
* Bad, Both work in the UI, and backend services
* Well known
* Good, large amounts of data

<!-- markdownlint-disable-file MD013 -->