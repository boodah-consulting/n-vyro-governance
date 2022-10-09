# 3 - Firmware Stack

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 09/10/2022, 03:08:16 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

We need to establish a standardised way of building our firmware, for our
custom microcontrollers.

This starts with what technology to use, and is completed by how do we generate
the appropriate binaries once we've stablished the designed technological
choices.

We need to be able to easily build C/C++ based firmware for our circuit boards.
Our microcontrollers are ESP-based, initially focusing on the ESP8266, although
the target chip is actually the ESP32. Development wise, there's not that much
difference between the two chips, however we want to be able to easily switch
between the two, or atleast be able to maintain a working build until we've
successfully managed to stabilise our ESP32 builds.

Hobby, and small projects tend to use ArduinoIDE, but we're not particularly
happy with using that within a production setting.

We need to have an ecosystem similar to our other programming languages, mainly
as context switching can slow down development, but also having similar setups
means that there are fewer things to go wrong.

We'll want to be able to test our code, we'll also need to be able to generate
the final binaries easily. Our firmware will focus purely on handling the lower
level logic, required to have a functioning n-vyro.io based device. However,
each piece of firmware will also need to be able to easily generate their
appropriate UIs.

## Decision Drivers <!-- optional -->

* ESP based
* C/C++
* Allows us to leverage our existing build tools
* Can generate final binary files (UI/Firmware)
* Easy to pick up
* Generate UI binaries
* Generate firmware binaries
* Testing
* CI

## Considered Options

* [ArduinoIDE](#arduionide)
* [(C)Make](#cmake)
* [PlatformIO](#platformio)

## Decision Outcome

Chosen option: "PlatformIO", because we are then able to leverage C/C++, and
stick to a development cycle that we're used to.

PlatformIO provides us with enough functionality to not only generate our
firmware binaries, but we can also develop in a very similar way as to how we
typically would when working in JS, or Ruby. This means that we can use the
tools that we're personally used too, but we can also tie our firmware into our
Ci pipeline.

Doing this we can further extend this functionality, so that we can update
_all_ devices, connected to the network. This makes it super easy to update an
ecosystem with the latest changes.

As this is a CLI tool, and rans under Python, we can also test against builds,
leading us towards being able to write super clean code, which is closer to
C/C++ than ArduinoC. It also provides us with an easy way to cobble together
some scripts for pulling down the correct UI, and generating the correct files.

The data structure is closer to that of a standard C/C++ project, so it
_should_ be relatively easy for others to pick up.

### Positive Consequences <!-- optional -->

* Easy to use
* Well known within the community
* Can use existing Arduion libraries
* Ties into C/C++
* Can pin version of ESP
* Allows us to build against multiple architectures

### Negative Consequences <!-- optional -->

* Requires more C/C++ knowledge
* Requires Python (to run via CLI)

## Pros and Cons of the Options <!-- optional -->

### ArduionIDE

https://www.arduino.cc/en/software

* Good, Most well known
* Good, Easy to use
* Bad, Clumsy interface
* Bad, Harder to test
* Bad, Harder to build
* Bad, Harder to debug
* Bad, Harder to update firmware

### (C)Make

https://cmake.org/

* Good, Known by most C/C++ developers
* Good, All the power
* Bad, Esoteric
* Bad, Requires deep knowledge

### PlatformIO

https://platformio.org/

* Good, CLI
* Good, Easy to update firmware
* Good, Build against multiple architectures
* Good, Customisable
* Good, Used by a decent amount of users
* Good, Pin versions
* Bad, More dependencies

<!-- markdownlint-disable-file MD013 -->