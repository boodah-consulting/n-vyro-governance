# 15 - Alert Payload

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 29/11/2022, 06:23:44 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

We've discussed [Message Payload](0002-message-payload.md) and [Notification
Payload](0014-notification-payload.md). Both discuss mostly implemented
messaging functionality.

Here we need to go over what the payload for an alert is. We will go into the
alert types, what they are for, and why we might use them.

User-generated triggers are the source of an alert message. Each tracks a
specific sensor value. So we will need to consider how to interact with the
system and the impact of our decisions.

## Decision Outcome

Here we discuss the varying payloads that an alert can have, helping to inform
other [Message Payload](0002-message-payload.md) within our [Messaging
Layer](0001-messaging-layer.md).

We know that a device can have several triggers assigned to it, each defining
the conditions of a specific sensor value (temperature).

For this reason, we have previously defined a structure for these
[topics](0013-messaging-topics.md). We have also discussed the payloads for
[notifications](0014-notification-payload.md).

These alert messages are already part of the ecosystem, so here we will go into
the details of each payload type, along with an example.

### Exact

Sent any time the incoming message matches a trigger with the "exact"
"humidity" expected value of 67.4873.

The incoming message must be _exactly_ the same as the trigger value.
Otherwise, the value does not match the value.

```json
{
  "topic": "alerts/shelf-1/humidity",
  "payload": {
    "id":"n-vyro-zone-0DE9E6",
    "type": "humidity",
    "state": "device",
    "on": "exact",
    "expected": 67.4873,
    "actual": 67.4873,
    "timestamp":1669703638
  }
}
```

### Above

Sent any time the incoming message matches a trigger with the "above"
"humidity" expected value of 40.

The incoming message must be _above_ the same as the trigger value. Otherwise,
the value does not match the value.

```json
{
  "topic": "alerts/shelf-1/humidity",
  "payload": {
    "id":"n-vyro-zone-0DE9E6",
    "type": "humidity",
    "state": "device",
    "on": "above",
    "expected": 40,
    "actual":67.4873,
    "timestamp":1669703638
  }
}
```

### Below

Sent any time the incoming message matches a trigger with the "below"
"humidity" expected value of 70.

The incoming message must be _below_ the same as the trigger value. Otherwise,
the value does not match the value.

```json
{
  "topic": "alerts/shelf-1/humidity",
  "payload": {
    "id":"n-vyro-zone-0DE9E6",
    "type": "humidity",
    "state": "device",
    "on": "below",
    "expected": 70,
    "actual":67.4873,
    "timestamp":1669703638
  }
}
```

### Outside

Sent any time the incoming message matches a trigger with the "outside"
"humidity" expected range of 40-60.

The incoming message must be _outside_ the same as the trigger value.
Otherwise, the value does not match the value.

```json
{
  "topic": "alerts/shelf-1/humidity",
  "payload": {
    "id":"n-vyro-zone-0DE9E6",
    "type": "humidity",
    "state": "device",
    "on": "outside",
    "expected": { 
      "min":40,
      "max":60
    },
    "actual":67.4873,
    "timestamp":1669703638
  }
}
```

### Inside

We're aware that we likely need an alert type of "inside", this hasn't been
implemented yet, but we add an example here as it has a similar structure to
the other alerts.

Sent any time the incoming message matches a trigger with the "outside"
"humidity" expected range of 40-60.

The incoming message must be _outside_ the same as the trigger value.
Otherwise, the value does not match the value.

```json
{
  "topic": "alerts/shelf-1/humidity",
  "payload": {
    "id":"n-vyro-zone-0DE9E6",
    "type": "humidity",
    "state": "device",
    "on": "inside",
    "expected": { 
      "min":40,
      "max":60
    },
    "actual":57.4873,
    "timestamp":1669703638
  }
}
```

Depending on the type of trigger, we will see changes to the "type", "on",
"expected", and "actual" values. Whenever an incoming message matches the
conditions of a trigger, an alert message will be published. We can rely upon
subscribing to this topic to ensure we're getting real-time alerts for a
device. 

## Next Steps <!-- optional -->

* Implement this payload

### Positive Consequences <!-- optional -->

* Payload is consistent
* Allows for improvement
* Allows for additional types

## Links <!-- optional -->

* [ADR-0001](0001-messaging-layer.md) - Messaging Layer
* [ADR-0002](0002-message-payload.md) - Message Payload
* [ADR-0013](0013-messaging-topics.md) - Messaging Topics
* [ADR-0014](0014-notification-payload.md) - Notification Payload

<!-- markdownlint-disable-file MD013 -->
