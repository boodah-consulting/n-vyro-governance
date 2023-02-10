# 14 - Notification Payload

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 29/11/2022, 05:16:43 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

We have several notifications within the ecosystem that make up our
[Messaging Layer](0001-messaging-layer.md). We've spent some time looking into
the structure of these [Messaging Topics](0013-messaging-topics.md). So now we
need to spend some time documenting how notifications are published.

Similar to our [Message Payload](0002-message-payload.md), we will highlight
the different message notifications and then go into detail about what each
payload looks like, informed by how we define messages within the [Messaging
Layer](0001-messaging-layer.md).

## Decision Outcome

Here we discuss the varying payloads that a notification can have, helping to
inform the other messages within our messaging layer. We already have [Message
Payload](0002-message-payload.md), and we've already implemented notifications.

Below we go into the details of each notification payload, along with their
purpose.

### Device State

Notifications are published whenever a device state has changed.

```json
{
  "topic": "notifications/shelf-light-1",
  "payload": {
    "type": "device",
    "state": "state",
    "payload": {
      "deviceID": "n-vyro-zone-ABC123",
      "state": true
    }
  }
}
```

### Hub

Anytime there is a change in the state of the NVyroHub daemon. These
notifications are then published. They are used to help determine when the
service has started or restarted.

We've already integrated these notifications into our NVyroHub daemon.
Although, these are only within the messaging daemon.

#### Starting the hub

```json
{
  "topic": "notification/n-vyro-hub",
  "payload": {
    "type": "daemon",
    "state": "start",
  }
}
```

#### Stopping the hub

```json
{
  "topic": "notification/n-vyro-hub",
  "payload": {
    {
      "type": "daemon",
      "state": "stop",
    }
  }
}
```

We also cater for daemon errors and interrupts. Here we will attempt to catch
any issues and publish them for debugging.

We've not integrated these notification types into the system, but we do
foresee these notification types to be triggered anytime there is an issue with
our NVyroHub service. Unlike the previous notifications, we expect these to be
within all NVyroHub-based services.

#### An error occurs

```json
{
  "topic": "notification/n-vyro-hub",
  "payload": {
    "type": "daemon",
    "state": "error",
    "payload": {
      "message": "Something went wrong"
    }
  }
}
```

#### Service interrupt

```json
{
  "topic": "notification/n-vyro-hub",
  "payload": {
    "type": "daemon",
    "state": "interrupt"
  }
}
```

### Messaging

We don't currently have this integrated, but the idea behind these
notifications is that they are published anytime NVyroHub connects or
disconnects from the messaging layer. It is rare for these notifications will
be triggered, so we've left them out for the time being.

Regardless, we've documented them here. So we can review their utility within
the ecosystem at a later date.

Allows all devices to get real-time updates on whether there is a connection to
the messaging layer. These will not only help with UX but also helps us to
debug issues with an NVyroHub device.

We've yet to integrate this into the ecosystem.


#### Connect to messaging layer

```json
{
  "topic": "notification/n-vyro-hub",
  "payload": {
    {
      "type": "messaging",
      "state": "connect"
    }
  }
}
```

#### Disconnect to messaging layer

```json
{
  "topic": "notification/n-vyro-hub",
  "payload": {
    "type": "messaging",
    "state": "disconnect"
  }
}
```

### Release

Provides a mechanism for informing a user that a new release version has come
out. We've yet to implement this functionality, but we've at least created the
notification type so that we can generate the notification when the time comes.

We envisage a separate service, which subscribes to this topic, and then
updates the list of available versions.

We publish new release notifications. As n-vyro.io has several packages, all of
which create our final product, it is crucial to have a way of notifying a user
of new changes.

We've implemented this notification so that we can create a service to handle
the specifics around version management.

#### NVyroHub release

```json
{
  "topic": "notification/n-vyro-hub",
  "payload": {
    "type": "version",
    "state": "update",
    "payload": {
      "version": "1.2.0",
      "package": "n-vyro-hub"
    }
  }
}
```

#### NVyroUI release update

```json
{
  "topic": "notification/n-vyro-soil",
  "payload": {
    "type": "version",
    "state": "update",
    "payload": {
      "version": "1.2.0",
      "package": "@n-vyro/frontend-ui"
    }
  }
}
```

## Next Steps <!-- optional -->

* Implement this payload

## Links <!-- optional -->

* [ADR-0002](0002-message-payload.md) - Message Payload
* [ADR-0013](0013-messaging-topics.md) - Messaging Topics

<!-- markdownlint-disable-file MD013 -->
