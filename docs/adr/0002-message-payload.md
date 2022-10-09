# 2 - Message Payload

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 09/10/2022, 02:00:05 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

When sending device messages we will need to have a consistent format, which can be used to inform the system about a
devices environment, state, or change something.

Dependant on the device, we will require a slightly different payload. Each of the following devices will have its own
payload structure:

* NVyroSense
* NVyroConnect
* NVyroHub

NVyroSense devices come in two flavours, NVyroSoil, and NVyroZone. The only difference between these two, are that
NVyroSoil includes moisture data, where both devices tracks temperature, and humidity.

Each device will include a timestamp. This will be formatted using the epoch format (1665277815), it will also be set
to GMT, so that we know that we have a consistent timestamp. Any timezone changes will need to be via the UI, and the
UI only. We don't want to have to think, or handle time variations, so it is simpler to just handle GMT and convert
from there.

The last part is the topic, this will be prefix with either `/sensors`, `/plants`, or `/listeners`

## Decision Drivers <!-- optional -->

* Already have an existing format
* Know that we've mixed up `/sensors` and `/plants` topics
* Must be as small as possible

## Considered Options

* include device name
* include device ID

## Decision Outcome

Chosen option: "include device ID", because we've already established that this is unique, so we can rely on the fact
that this will also refer to the correct device. We chose to "send whole payload", as although this gets close to our
payload limit, it saves our devices sending out more messages than required.

It is totally possible to include the device name, however we need to consider that our message payloads are already
quite large, we can work this out by looking up our sensor data, and mapping it to our device ID.

As we've already implemented this, along with mixing up `sensors`, and `plants`, we need to accept this piece of
confusion. We _do_ want to correct this, but making this change would include makes changes throughout the whole
system. So we will likely do this as part of a _major_ release, until then, we will accept this isn't ideal and not
force the change in between a QoL release.

This will give us the following types of messages.

### NVyroSense

Regardless of whether we have an NVyroSoil, or NVyroZone, device. Both include the topic prefix of `plants`. Our UI
listens out for these messages, so that we can retrieve real-time updates. As these devices were originally designed
to monitor plants, the prefix has stuck around, and is already being used within existing systems.

We're aware that the topic prefix is somewhat confusing, in an _ideal_ world, we would name this something like
`sensors`, as these are technically sensor devices.

```json
{
  "topic": "plants/my-chilli-pot",
  "payload": {
    "deviceID": "n-vyro-zone-ABC123",
    "temperature": 23.0,
    "humidity": 55.0,
    "timestamp": 1665277815,
  }
}
```

### NVyroConnect

These devices solely handle the state of the connected device. As this is the case, the device simply sends out a
message payload telling the ecosystem what state it is currently in. We had thought of these devices as "sensors", so
similarly to the topic of NVyroSense, we have stuck with this prefix, until we have the chance to safely change them.

We've not _formally_ come up with a preferred prefix, but we'd suggest something that is intuitive, potentially
`switches`, or `connects`

```json
{
  "topic": "sensors/my-chilli-lights",
  "payload": {
    "deviceID": "n-vyro-connect-ABC123",
    "state": true,
    "timestamp": 1665277815,
  }
}
```

### NVyroHub

Similarly to NVyroConnect this device sends state data.

As there is _typically_ only one NVyroHub, per ecosystem, we've decided to statically define the name. Here we're
sending the data to an NVyroConnect, telling it to change its state. The other difference is that we include the event
type that triggered the action, this is helpful for debugging eventing issues.

Here, the topic is just fine, it does what it says on the tin. It's sending a message to the `listener`
`my-chilli-lights`.

```json
{
  "topic": "listeners/my-chilli-lights",
  "payload": {
    "deviceID": "n-vyro-hub",
    "state": true,
    "type": "temperature",
    "timestamp": 1665277815,
  }
}
```

### Positive Consequences <!-- optional -->

* Sends all metrics
* Makes use of the device ID

### Negative Consequences <!-- optional -->

* Payload can be large (128+ bytes)
* Confusing topic prefixes (except NVyroConnect listener topic)

## Links <!-- optional -->

* [ADR-0000](0000-device-naming-convention-(deviceid).md) - Device Naming Convetion (DeviceID)
* [ADR-0001](0001-messaging-layer.md) - Messaging Layer

<!-- markdownlint-disable-file MD013 -->