# 16 - Settings Payload

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 29/11/2022, 13:57:30 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

In [Messaging Topics](0013-messaging-topics.md), we mentioned that we need a way of notifying the ecosystem whenever
the settings of a device have changed. Since values like "name" directly change a device's topic and subscription
address. When these are changed, there are parts of n-vyro.io that will need to be aware of these changes so that they
can be updated automatically.

We've already implemented a basic version of this. It has yet to integrate into the ecosystem. So we need to formalise
what exists.

We publish settings messages to the `notifications/devices` topic, meaning we'll need to update this before we fully
integrate our [Notification Payload](0013-notifications-payload.md) [Alert Payload](0014-alerts-payload.md).

Not doing so will mean that any changes to a device's settings will publish to the `notifications/devices` topic,
which will conflate notifications with settings messages.

## Decision Drivers <!-- optional -->

* Topic conflicts [Notifications Payload](0013-notification-payload.md)
* Lots of duplication in the payload

## Considered Options

* Leave topic as is
* Include device id in topic
* Include the device name in the topic
* Custom topic for all updates

## Decision Outcome

Chosen option: "Custom topic for all updates", because these messages are infrequent, and there's a low chance of
multiple devices are updated. We'll need to be able to filter out these messages if we want to use them for
notifications.

We've already decided on the payload for these settings changes. Below we provide an example:

```json
{
  "topic": "settings/devices",
  "payload": {
    "original": {
      "name": "My Device",
      "mqtt_topic": "plants/my-device",
    },
    "changed": {
      "name": "foo bar",
      "mqtt_topic": "plants/foo-bar",
    },
  },
}
```

There's little reason to publish the whole of both versions, as we're only interested in the values that have changed.
We ensure that we're sending both the original and the changed values. 

We need to be mindful that because we're changing the device settings, we're also possibly changing the device topic,
subscription, or name, due to not being able to rely upon the values we would use to determine which device is sending
a message. So, we can't use the device's name, id, or any identifier typically used.

As we've already used the `alerts` and `notifications` topic prefixes, it makes sense to prefix this topic with
`settings`. We don't see any other situation where we'd need to use this prefix anywhere else, so we should be safe to
use it here. Multiple devices will rarely update their settings at the same time. So we've decided to publish all
messages to `settings/devices`. Even if this happens, as this is our messaging layer, our services _should_ pick up
these messages individually. If this isn't the case, we suggest debugging these messages to ensure we can handle
publishing multiple messages to a topic.

We suggest that there is a separate service for listening out for these changes, and this service should _only_ focus
on updating all data that is associated with the device settings.

## Next Steps <!-- optional -->

* Implement this payload (@n-vyro/bootstrap-frontend)
* Determine services required to update devices on the `settings/devices` topic
* Implement services for listening out for `settings/devices` topic

### Possible Consequences <!-- optional -->

* Smaller payload

### Negative Consequences <!-- optional -->

* Miss device settings change messages
* Too many messages to handle

## Links <!-- optional -->

* [ADR-0002](0002-message-payload.md) - Message Payload
* [ADR-0013](0013-messaging-topics.md) - Messaging Topics
* [ADR-0013](0013-notification-payload.md) - Notification Payload
* [ADR-0014](0014-alert-payload.md) - Alert Payload

<!-- markdownlint-disable-file MD013 -->
