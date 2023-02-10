# 13 - Messaging Topics

* Status: draft <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 28/11/2022, 14:20:14 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

We must have a standardised way of handling the different topics within our messaging layer. Initially, we only
assigned topic addresses to NVyroConnect and NVyroSense. These topics, creating our [Messaging
Layer](0001-messaging-layer.md).

These included messages from NVyroConnect with the topic prefix `sensors` and NVyroSense devices with the `plants`
prefix. These prefixes aren't ideal, so we should look to replace them. However, we will need to investigate the
impact of these changes and plan to migrate to the preferred prefix.

We also want to ensure that our newly introduced topics follow a similar pattern to our existing addresses. Alerts, in
particular, can have multiple messages for a single device, so this will need a bit more consideration as we do not
overwrite these messages accidentally.

We also publish a message to `notifications/devices`. These messages notify NVyroHub of the device changes and make
the appropriate changes. We may need to modify this and consider other notifications we are yet to implement. With
this exploration, we should then be able to determine a topic address pattern and describe its utility.

Here we'll only go into the topic structure, addressing each payload within separate ADRs.

## Considered Options

* Nested topics
* Hyphenated topics
* Combination of the above

## Decision Outcome

Chosen option: "Nested topics". As it allows us to create topic addresses based on a set of pre-defined patterns. It
should also give us scope to make changes in the future.

We know that there are at least another two custom devices to enter the ecosystem, so we've also factored this into
our thinking.

### ESP-based devices

All [ESP-based](0005-custom-circuit-technical-details.md) devices will begin with the device type as a prefix,
followed by the device name. The only rule here is that the device name must be unique within the ecosystem, as our
notification topic addresses must not collide.

The [UI Stack](0004-ui-stack.md), [Data Layer](0008-data-layer.md) and [API Stack](0009-api-stack.md) need to be aware
of this, as this is where we handle management, validation and checks. When a device's name changes, we need to
validate that it doesn't conflict with another device's name.

Examples:

* `switches/shelf-lights-1`
* `sensors/shelf-1`
* `cameras/shelf-cam-1`
* `probes/water-tank-1`

Whenever a device's settings change, a notification is published. As it is possible to change a device name, we handle
to be able to these changes here. Currently, this notification has the prefix `notifications/devices`.

### Settings change

However, we suggest replacing this prefix with the following `settings/devices` name. As this is specifically for
settings change, and these happened pretty infrequently. So we should be free to use the topic address to handle all
our device changes.

Examples:

* `settings/devices`

### NVyroHub

An NVyroHub sends and receives a large number of messages.

Its sole purpose is to listen out for incoming ESP-based device messages and to check whether a responding message
needs to be published. These notifications can come in the form of a state change to NVyroConnect, or generating
alerts or notifications. All of which are triggered by user input.

Depending on the notification type, the payload will differ. However, the topic follows the structure described below.

#### NVyroConnect

The topic includes a parameterised version of the device name.

These messages are sent out continuously by NVyroHub, regardless of whether the state has changed. These messages keep
the device in the expected state. As an incoming message include this, it is easy to determine via an incoming
message.

Example:

* `listeners/shelf-lights-1`
* `listeners/shelf-fan-1`
* `listeners/shelf-humidifier-1`

#### Alerts

Alerts are generated based on user input, triggers specifically. As there can be overlapping alert messages, it's wise
to split the topics by their device name and type. This will prevent them from overlapping with each, and ensure that
all alerts are captured in a timely manner.

Example:

* `alerts/shelf-1/temperature`
* `alerts/shelf-1/humidity`
* `alerts/shelf-1/moisture`

#### Notifications

These can come in two flavours: system-based and event-based.

Event notifications always have a payload from the associated incoming message. Whereas system notifications only
provide a state, a type, or sometimes a notification payload.

We will likely need to do similar for events to ensure they don't overlap.

Examples:

* `notifications/shelf-1`
* `notifications/shelf-lights-1`
* `notifications/water-tank-1`
* `notifications/shelf-cam-1`
* `notifications/n-vyro-hub`

### NVyroConnect

Currently, we're using the `sensors` topic prefix. Due to our rapidly developing NVyroSense, we didn't have the space
to consider the name.

NVyroConnect connects to a 12/24v DC output, typically working as an automated switch. So it makes sense to replace
this with `switches`. As mentioned previously, we won't be changing this immediately, as we need to factor in the
impact of the change and how best to implement it.

### NVyroSense

Similarly to NVyroConnect, this topic was created when we were prototyping the system. Originally these were only
NVyroSoil devices, so we assigned them with the prefix `plants`. We've moved away from this concept now. Regardless of
whether it's NVyroSoil or NVyroZone, these devices are sensors, which is the reason is that we've decided on the
prefix `sensors`.

### NVyroHub

This device sends out both notifications and alerts. Both of these are generated based on user-generated input.

A device can have several triggers, so we need to consider this for our address pattern. These will look like
something like this: `alerts/shelf-1/humidity`.

If we set temperature, humidity and moisture triggers for "Shelf 1", NVyroHub sends the appropriate alerts. Then it
will send messages to the following topics: `alerts/shelf-1/temperature`, `alerts/shelf-1/humidity`, and
`alerts/shelf-1/moisture`.

For a notification, we handle things a little differently. Firstly we envisage an eventing system that checks whether
an update or new event does not conflict with another.

With these rules in place, it stands to reason that we will only receive a single notification for any given device.
With this in mind, it makes little sense to structure our topics similarly to our alerts. So these topics will have
the following structure `notifications/shelf-1` and `notifications/n-vyro-hub`.

### Other Devices

We know that there will be a time when we introduce new devices, and for this reason, we've spent some time
considering how these devices would fit into our topic structure.

Two types of devices we'll be introducing soon are NVyroEye and NVyroProbe.

NVyroProbe will likely be the first, essentially sending periodic updates of its state/stats. NVyroEye will be a
camera-enabled device, we've not spent much time exploring this yet, so it's hard to determine what kind of data it
would send out. It will likely listen out for messages, similar to NVyroConnect.

Nevertheless, with both devices, we will conceivably be sending out real-time data, and we'll want topic prefixes to
differentiate between the two.

NVyroProbe has the prefix `probes`, whereas NVyroEye has the prefix `cameras`.

So, if we had an NVyroProbe device called "Water Level" and an NVyroCam called "Shelf Cam 1", then we would have the
topics `probes/water-tank-1` and `cameras/shelf-cam-1`, accordingly.

We need to make sure that these names are unique within their device type, and we'll also need to make sure that we
have a mechanism for updating these whenever the user-friendly names change.

### Positive Consequences <!-- optional -->

* Each message has a unique topic
* It is easy to split the topic address
* Both alerts and notifications can be listened to independently
* Easy to determine the message type

### Negative Consequences <!-- optional -->

* May end up clobbering settings messages

## Links <!-- optional -->

* [ADR-0001](0001-messaging-layer.md) - Messaging Layer
* [ADR-0002](0002-messaging-payload.md) - Messaging Payload

<!-- markdownlint-disable-file MD013 -->
