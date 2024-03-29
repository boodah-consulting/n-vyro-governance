# 5 - Custom Circuit Technical Details

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 09/10/2022, 06:00:22 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

Our custom circuits are created to allow us to provide users with simple, and
intuitive smart devices. We currently have 3 custom circuits, however 2 of
those have a lot of similarity (tracking environmental data).

To create the brains behind these devices we will use a mix of our [UI
Stack](0004-ui-stack.md) and [Firmware Stack](0003-firmware-stack.md). Each
stack is modular in design. So we'll want to make sure that this layer also
addopts this principle. Re-using as much logic as possible.

The main different between devices, from an application level, is that
NVyroSoil collects moisture data, whilst both NVyroSoil, and NVyroZone collect
humidity, and temperature. NVyroConnect is functionality very different, but
from an architectural perspective they all share a lot in common.

It will be important that all of our custom circuits have exactly the same
bootstrap sequence, and equally we will want to make sure that users are able
to set up, and use, any of our device in exactly the same way. Really, what we
want here is for all devices to look the same, with exception to what the core
function of the device is, and the brand name.

This lead to us extracting out a bootstrap, for both the UI, and the firmware.
Meaning that both stacks have a private library which this layer will heavily
rely upon. These libraries only deal with initialising our devices, so it will
be vital that we keep this boundary clean.

If theres a new device introduced into the ecosystem we would need to create a
new device component, custom read/write logic and then integrate it into the
system. This way it is easier to maintain these clear boundaries. This logic
should feed into our existing layers to create a new device. Obviously there's
a little bit more to this, but the aim is to have a truly modular device, that
are intuitive to use.

## Decision Drivers <!-- optional -->

* Custom device ID
* Re-useable UI
* Shared bootloader UX
* Functionality overlap
* Able to send, and receive messages
* Works with all future devices

## Considered Options

* Monorepo
* Scripts for UI generation
* private dependencies
* Shared library
* Modular Architecture

## Decision Outcome

Chosen option: "private dependencies", "Scripts for UI generation", and
"Modular Architecture", because we're very comfortable extracting common
functionality into separate packages.

It is totally feasible to handle all of our code within a "Monorepo", however
this would lead to making it easier for us to blur the modular boundaries we're
trying to maintain. With a bit more work we could make a "Monorepo" achieve
this, but it takes more discipline, and we're not willing to enforce that, not
at this time, so a low tech approach will suffice. We want to be able to focus
on one thing, and one thing only. This also includes not having to interact
with languages that you have no need of interacting with.

It is for this reason that we ended up using a private library for handling the
commonly used code. All of our custom circuit firmware makes use of a C/C++
library for handling the shared functionality between all of our n-vyro.io
based devices. It defines the functionality needed for connecting a device to a
Wi-Fi network, along with allowing users to store their user settings. We also
included logic for handling the displays on these devices. We need to do some
work to improve upon this, but its working well so far. We'll come back to this
area once we've acquired more C/C++ knowledge. Even though this isn't an ideal
solution. It would not take much effort to refactor this to take create shared
objects. Perhaps this could be done in something like Go, or Rust. We lack
resources, at present, but it would be cool to explore this at a later date.

Each device shares logic for interacting with our [Messaging
Layer](#0001-messaging-layer.md), they all need to be able to communicate via
these layers, and we want messaging to be consistent through our devices. This
means that [Message Payload](#0002-message-payload.md) is referred to make sure
that we are sending messaging with the expected format.

As this is common functionality it will be part of the "Private Dependencies",
for both the core firmware, and the UI, so we need not know anything about how
the messages are being sent, we just have to handle the logic for sending the
final message. This allows us to create projects that only focus on the thing
they care about, and moves all the non-specific implementation out to the
appropriate home.

Generating the appropriate static files can be really cumbersome. So we need to
formalise this, and turn our manual steps into something automated.

Even though VueJS gives us a script for generating these files, they aren't
quite in the correct format for our firmware to handle. We also need to
consider the file limit that we have on our microcontroller. This is limited to
4MB, which includes the firmware and UI binaries.

Our [UI Stack](0004-ui-stack.md) includes this functionality, so we will
leverage this for doing the initial compilation. Our custom devices currently
have really simple interfaces, so there's little need to break those out into
individual packages. We can leverage our scripting skills to determine which
device we want to generate a UI for, and specify it.

The stack is already broken down enough, so everything within the core UI
package is purely to handle the UX for these devices. One limitation to our
current setup is we're forced to use SPIFFS, ESPs original filesystem. This
limits the length of our filenames. Due to this we need to massage our files,
so that they'll fit properly. Each firmware project has a script for generating
the static HTML files.

Example: `./bin/generate_ui --ui v1.7.4 --firmware v1.11.0 --path /path/to/project
--type n-vyro-zone`.

This script takes the version of the firmare, and the UI, and pulls down the
appropriate UI package. It will then generate the static HTML needed for the
devices UI.

(As this script is pretty generic, we've supplied a
[template](../templates/firmware/bin/generate_ui) script for new projects).
Once this is complete we're ready for PlatformIO to take over and upload the
firmware. Although this has been done for NVyroSoil, NVyroZone, and
NVyroConnect, there's no reason why future devices can not follow this pattern.
The new device would have to write its own custom logic, but with some skimming
through the codebase of the existing firmware, it will not be much effort to
repeat this pattern.

Similarly, as we have very similar operational requirements, we will re-use the
scripts we've used to generate, and release our firmware. This can be found
within the firmware [template](../templates/firmware/bin) directory.

Each script has a `PACKAGE_NAME` variable, this will need to be updated, once
the scripts have been copied across.

### Positive Consequences <!-- optional -->

* Can re-use the library
* Another dependency
* More code to maintain
* Re-uses existing tools
* Extendable
* Easy to add new devices

### Negative Consequences <!-- optional -->

* Private, internal, dependency
* More code to maintain
* Knowledge from multiple disciplines

## Links <!-- optional -->

* [ADR-0000](0000-device-naming-convention-\(deviceid\).md) - Device Naming Convention (DeviceID)
* [ADR-0001](0001-messaging-layer.md) - Messaging Layer
* [ADR-0002](0002-message-payload.md) - Message Payload
* [ADR-0003](0003-firmware-stack.md) - Firmware Stack
* [ADR-0004](0004-ui-stack.md) - UI Stack

<!-- markdownlint-disable-file MD013 -->