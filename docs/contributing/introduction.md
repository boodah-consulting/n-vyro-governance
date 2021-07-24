# Introduction

There's a lot here, so I appreciate that it's not clear where to start.

I've spent a lot of time inside of the Matrix, as it were, so it's a bit challenging to make this as comprehensive as I'd like. But this will be the starting point for fleshing out how others are introduced into the eco-system.

Over time, this will be fleshed out, so that it becomes a clear path for everyone to get started with n-vyro.io.

----

## Overview

Most users will just want to do one of the following:

* Test out eco-system
* Be able to update their devices
* Contribute and make various changes

## Test out the eco-system

There are currently 4 kinds of devices:

* NVyroHub
* NVyroConnect
* NVyroSensor
* NVyroSoil

### NVyroHub

NVyroHub is the brain of the ecosystem. It's used to create your growing environment and handle the events for each growing environment you set up.

For a more high-level breakdown of NVyroHub, you can go [here](https://n-vyro.io/product/n-vyro-hub/).

NVyroHub needs to be set up on a Raspberry Pi, for this, you can go to https://github.com/boodah-consulting/n-vyro-build for more information.

### NVyroConnect

NVyroConnect allows you to be able to connect switch-based devices via the n-vyro.io eco-system.

This is hardware-based, so you'll need to have a physical version of the device to see it working in the wild. However ... it is possible to get the UI running locally, for experimentation, development, and plain noisiness.

For a more high-level breakdown of NVyroConnect, you can go [here](https://n-vyro.io/product/n-vyro-connect/).

To look further under the hood we suggest that you check out [here](https://github.com/boodah-consulting/NVyroConnect/blob/main/README.md).

### NVyroSensor

NVyroSensor is an environment-based smart device, which allows you to track environmental data in the area that it is set up.

This is hardware-based, so you'll need to have a physical version of the device to see it working in the wild. However ... it is possible to get the UI running locally, for experimentation, development, and plain noisiness.

For a more high-level breakdown of NVyroSensor, you can go [here](https://n-vyro.io/product/n-vyro-sense/).

To look further under the hood we suggest that you check out [here](https://github.com/boodah-consulting/NVyroSense/blob/main/README.md).

### NVyroSoil

NVyroSoil is a soil-based smart device, which allows you to track environmental data in the area that it is set up.

This is hardware-based, so you'll need to have a physical version of the device to see it working in the wild. However ... it is possible to get the UI running locally, for experimentation, development, and plain noisiness.

For a more high-level breakdown of NVyroSoil, you can go [here](https://n-vyro.io/product/n-vyro-sense/).

To look further under the hood we suggest that you check out [here](https://github.com/boodah-consulting/NVyroSense/blob/main/README.md).

## Be able to update their devices

When it comes to NVyroHub that's pretty straightforward, head over to n-vyro-build and follow the instructions there hardware.

As for the custom devices, they depend on whether it's the UI of the hardware's firmware you want to update Regardless you can check out NVyroConnect or NVyroSensor for examples of how to do either.

## Contribute and make various changes

You've likely found something you'd like to change, or you've thought of cool addition to the ecosystem, this doesn't have to be technical either. We encourage improvement across the whole spectrum, whether that be improvements to the documentation or how we prioritise change.

Discussions are open for those that have ideas and want to collectively flesh them out. Ideas that hold up will then be filtered into the changelog and anyone is open to implementing the requested changes.

If it's a code change like documentation improvements or bug fixes, it can be handled via a PR, reviewed, and then introduced into the codebase.
