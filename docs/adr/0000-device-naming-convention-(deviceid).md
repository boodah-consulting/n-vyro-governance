# 0 - Device Naming Convention (DeviceID)

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 09/10/2022, 00:22:19 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

As we can have a number of devices within the ecosystem, it is not enough to simply to display the type of device
we're working with. We also need a unique identifier, to help distinguish between devices of the same type.

As each device have a user defined name, this can be changed at anytime. So we can't rely upon this when we want to
associate a device to something.

A device ID will allow us to have a dependable identifier for each device, whilst also providing us with the ability
to associate a piece of data to a device. 

This will allow us to reliably identify a device, and leave the user to change the devices name without having to
worry about anything unexpected happening.

## Decision Drivers <!-- optional -->

* Need to easily identify a device
* Done on firmware layer
* Must be unique
* Will be used to associate a device with existing data
* Ideally never changes

We know we want to have a prefix of 'ABC123', this will be a generated 6 character hex. This will provide us with a
combination of 17,777,216 unique identifiers.

The unanswered part is what do we prefix this identifier with?

## Considered Options

* [brand name - device type](#brand-name---device-type) - (n-vyro-zone-ABC123)
* [brand name](#brand-name) - (n-vyro-ABC123)
* [user defined name](#user-defined-name) - (my-custom-device-ABC123)

## Decision Outcome

Chosen option: [brand name - device type], because this allows us to easily determine the type of device
we are working with.

This was decided due to the fact that an ecosystem can have a number of devices within it. Only prefixing the brand
name, as with option 2, would mean that it would be harder to discern between device types. Leading to extra time
debugging, and other headaches.

Alternatively, if we used option 3, then we would be forced to update all associated data, whenever the device name is
changed. This was cause a hell of a lot of extra work, as well as introduce bugs whenever a device is updated.

This means that we would end up making the ecosystem more complex than it needs to be. We also don't want to deal with
the debugging of such a designed system.

Options 1 keeps things nice and simple.

The device generates the ID, and then we simply refer to it. As the device ID never changes, we can safely rely upon
using it as a reference.

The only time we'd expect a similar char, would be if we had installed the firmware of another device type. IE. an
NVyroZone device is updated with the NVyroSoil firmware.

So that would be more than enough for a single ecosystem, however we further increase our uniqueness when we add the
device type prefix.

We can leverage the ESP here, by using `ESP.getChip()`. This functionality provides us with a 6 character hex, which
is based off of the architecture of the microcontroller. This will provide us with our randomness, removing the need
for us to create "yet another" random generator, and makes sense as each chip should have its own unique ID.

Combined, this creates a unique device ID (i.e. n-vyro-zone-ABC123).

Each of our ESP-based devices generate this ID, in the same, way, so we are rest assured that each device provides this
functionality out of the box.

### Positive Consequences

* 17,777,216 unique identifiers, per device type
* Easy to discern device type
* Uses `ESP.getChip()` to generate the 6 character hex
* Never changes

### Negative Consequences

* name takes up more bytes
* Ties in brand name (needs changing, if brand changes)

## Pros and Cons of the Options

### brand name - device type

[n-vyro-zone-ABC123 | An NVyroZone device | Generates zone sensor based metrics]

[n-vyro-soil-ABC123 | An NVyroSoil device | Generates soil sensor based metrics]

[n-vyro-connect-AEC321 | An NVyroConnect | Listens out for NVyroHub messages]

* Bad, More bytes used in message bus payload
* Bad, depends on brand name (if changes)
* Good, depends on brand name (brand recognition)
* Good, Easy to distinguish between devices
* Good, Never changes

### brand name

[n-vyro-ABC123 | An NVyroZone device | Generates zone sensor based metrics]

[n-vyro-ABC123 | An NVyroSoil device | Generates soil sensor based metrics]

[n-vyro-AEC321 | An NVyroConnect | Listens out for NVyroHub messages]

* Bad, Unable to determine device type
* Bad, Potential of hex character conflict
* Good, The Least amount of message bytes
* Good, Never changes

### user defined name

[grow-shelf-1-ABC123 | An NVyroZone device | Generates zone sensor based metrics]

[chilli-pot-ABC123 | An NVyroSoil device | Generates soil sensor based metrics]

[shelf-lights-AEC321 | An NVyroConnect | Listens out for NVyroHub messages]

* Bad, Need to escape user input
* Bad, need to update when user changes the name
* Bad, Forces us to update the device ID, anytime the name changes
* Bad, Will need to create a default name (factory settings up)
* Bad, not sure what the device type is, could infer
* Good, human-readable

<!-- markdownlint-disable-file MD013 -->