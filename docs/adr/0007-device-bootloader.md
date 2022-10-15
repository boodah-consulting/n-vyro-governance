# 7 - Device Bootloader

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 15/10/2022, 23:32:31 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

It is important that our users are able to easily set up their device, so they
can communicate with each other. There are a number of ways in which they could
do this, but we want to make sure that this process is as intuitive as
possible, as well as being something that doesn't feel annoying when having to
set up multiple devices. This also needs to be so easy that anyone can use it,
meaning that we should use a process similar to what already exists.

## Decision Drivers <!-- optional -->

* Easy to set up
* Reliable
* Work for all custom devices
* Must be written as a firmware library

## Considered Options

* [ConfigManager](https://github.com/boodah-consulting/ConfigManager)
* [Matter](https://github.com/project-chip/connectedhomeip) (formerly CHIP)

## Decision Outcome

Chosen option: "ConfigManager", because we need these devices to communicate
with a network. Bluetooth would be cool to explore, but it seems like
[Matter](https://github.com/project-chip/connectedhomeip) will provide us even
more flexibility. Leveraging
[Matter](https://github.com/project-chip/connectedhomeip) would have been
ideal, but we would have had to create our own abstraction layer between that
layer and what ever we'd need to implement to allow users to connect to our
devices. This wouldn't just be time-consuming we would have likely introduced
unforeseen bugs. We know we want to further enhance this process, so there's
little need to making this perfect, and more importance in being able to get
the core functionality validated. We would love to explore integrating
[Matter](https://github.com/project-chip/connectedhomeip) into our ecosystem,
however this will likely be done once we've stabilised the rest of the system.

#### ESP Devices

[ConfigManager](https://github.com/boodah-consulting/ConfigManager) gave us
enough flexibility to allow use to turn the device into its own Access Point,
whenever the device couldn't connect, or wasn't setup. There was also a hook,
which allowed us to tie our UI into the workflow and create a simple journey
for boot loading a device. The library handles the storing of the Wi-Fi
credentials for us, and is nice enough to have a web server that we can
leverage for our own needs.

As we've already implemented this in our prototypes we're pretty comfortable
using it, and it won't take much effort to refactor this so that all of our ESP
based devices can use this workflow. With this functionality being shared with
all devices, all we want to do is define the settings, and initialise object.
Our shared code must do the bulk of the work. There is one side effect to using
this project, it is only available in C, and is focused on Arduino based
devices. This means that NVyroHub will not be able to leverage this. For the
time being, this is okay.

### NVyroHub

The hub is set up in a totally different way, and will be setup by users. This
is purely due to the fact that we need to explore ways of doing similar within
the Linux ecosystem. We provision the hub onto a
[RasperryPi](https://www.raspberrypi.com/software/), so we ask users to set up
their hubs using the installation instructions there. All we need them to do
from there is to log in to the server, set up the [configuration
file](https://github.com/boodah-consulting/n-vyro-build/blob/main/n-vyro/hub/inventory/example-arm64.yml),
and then run our provision tool. This will set up NVyroHub on their
[RasperryPi](https://www.raspberrypi.com/software/) and get the ball rolling.
From here users will be able to access their NVyroHub in the same was as they
can access our other devices.

## Links <!-- optional -->

* [RasperryPi](https://www.raspberrypi.com/software/)
* [ConfigManager](https://github.com/boodah-consulting/ConfigManager)
* [Matter](https://github.com/project-chip/connectedhomeip)
* [ADR-0003](0003-firmware-stack.md) - Firmware Stack


<!-- markdownlint-disable-file MD013 -->