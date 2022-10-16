# 10 - Common Core

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 16/10/2022, 02:58:31 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

It is important that we have a common vocabulary for the core aspects of
[n-vyro.io](https://n-vyro.io). This will serve as a central point of truth for
all aspects of [n-vyro.io](https://n-vyro.io) that are specific to the domain,
and not directly part of the [messaging](0001-messaging-layer.md),
[data](0008-data-layer.md), or [API Layer](0009-api-layer.md).

Typically, this will consist of objects which are shared throughout the whole
ecosystem and help to define the parts that naturally fit within the problem
domain. It will be important that this layer is easy to understand, and that it
is flexible enough to work with _all_ of the system. It is important that we
only define primate domain logic, and that they are _core_ to the ecosystem.

We already know that it would be ideal to have this as a shared object, but it
feels premature to jump straight to rewrite this logic into something like
C/C++. Partly as we've already tried this, and it can be quite complex, and
also because these structures aren't that complex, so duplicating them once
isn't too much of a big issue. However, as we endeavour to stick to having one
central point of truth, so eventually we will end up having to make this
decision.

For now, it is good enough to simply define this boundary, and then later, once
things have settled, we can come back and determine whether we need to take
things further.

Within the core, we will have the following concepts:

* Brand
* Product
* Time
* Device
* Alert
* Event
* Trigger

#### Brand

As our branding is everywhere it makes sense to have a place to define the core
aspects of our branding and leverage this to keep our codebase consistent.
These objects are central to all of our core compoents, so if we end up making
any branding changes, this is where we should start.

Example: https://github.com/boodah-consulting/n-vyro-core-ruby/blob/main/lib/n-vyro/core/brand.rb

#### Product

This layer helps us to define our products, and their defining attributes. It
allows us to have a central place for defining the name of each product within
our codebase.

This makes it easier for us to make changes to our product names easily, and
without having to make the change everywhere.

Example: https://github.com/boodah-consulting/n-vyro-core-ruby/blob/main/lib/n-vyro/core/product.rb

#### Time

Time is hard, and it can be painful to make sure that all devices are working
in the same way. One of the core parts where this is crucial is within our
event checking. We not only need to make sure that we are consistent with our
checks, but we also need to make sure that how we calculate things like
the current time is handled in one way.

It is here where we also define things like durations, time-based formats what
the current time is. This makes it easier to work with time, and means that if
we have to address any issues, we know exactly where to look.

Example: https://github.com/boodah-consulting/n-vyro-core-ruby/blob/main/lib/n-vyro/core/time.rb

#### Device

Each device sends and receives slightly different data depending on the device
itself, even though there are a lot of similiarities. To make it explicit what
each device is able to do, we define this logic here. Anytime we introduce a
new device into the ecosystem, we'd need to create a new definition here. This
is also where we define things like the devices topic patten, and its sensor
values.

Example: https://github.com/boodah-consulting/n-vyro-core-ruby/blob/main/lib/n-vyro/core/device/n_vyro_connect.rb

#### Trigger

These define the conditions that are required to create an alert.

Triggers are need to create an Alert. They are user defined, so we need to have
a place to define what a one is and what types are available.

Example: https://github.com/boodah-consulting/n-vyro-core-ruby/blob/main/lib/n-vyro/core/trigger.rb

#### Alert

Created whenever a triggers conditions are met. There are different types of
alerts, and they are dependent on the type of device we're creating a trigger
for. This means that we need a central place to define this logic.

We also need a place to define alert based constants, such as the expiration
length.

Example: https://github.com/boodah-consulting/n-vyro-core-ruby/blob/main/lib/n-vyro/core/alert.rb

#### Event

An event is another important part of our system. Similar to the other core
aspects of the system this is where we define the core aspects of the dataset,
along with what it is to be an event.

It is another part of the system that is user defined, so we use this as a way
of defining what an event is and how they work based off the assigned
device.

Example: https://github.com/boodah-consulting/n-vyro-core-ruby/blob/main/lib/n-vyro/core/event.rb

## Decision Drivers <!-- optional -->

* Easy to understand
* Well documented
* Establishes a clear boundary

## Considered Options

* C/C++
* Ruby
* Rust/Go

## Decision Outcome

Chosen option: "Ruby", because most of our core services are written in the
language. It is also expressive enough to easily describe our intent, whilst
clearly documenting its use.

Although it would be helpful to also have, at least, some of these objects
accessible via C/C++ it would take a lot of effort to maintain and update this.
Ruby gives us the added comfort of not having to worry about things like memory
management, and types, so we can simply run with it, and not have to worry
about such things.

Even though we've recently simplified how we store an area, it would still be
some undertaking to not just write this code in C/C++, but to also expose that
to our JS, and Ruby libraries.

Most of our complexity sits within our services, and we've got a pretty decent
level of separation there, so it feels like overkill to add an extra layer of
separation, without having the skill set to maintain it.

The other aspect, which would likely be easier to extract into a static object
would be our core library. This is mostly defining our domain logic, so there
is more declaration than computation there. However, we've not long implemented
this code, so feel it makes sense to allow it to mature before we reimplement
it.

Introducing a static object would mean that our core is truly reusable,
however, the main thing holding us back is our lack of expertise in
implementing this. It is for this reason that we've ended up defining all of
this logic in Ruby.

The main driving force for this is that we're more focused towards getting our
ecosystem in front of users, as opposed to making everything perfect. Once we
begin to gather some funds, and then we can invest in explore this better. It
is also beneficial to us as we've got a lot of experience with the language,
and can use this as a way of defining boundaries, whilst not getting bogged
down with the _ideals_.

Regardless of implementation, it is _this_ layer that will define the domain
specific logic and the data structures that can interact with it.

It will also be objects from this core than are heavily used throughout the
system. So we need to make sure that what we've implemented is solid, and is
consistent throughout the ecosystem. So once the core matures we should start
to explore extract out less performant aspects to another language. Likely
Rust, or Go.

### Positive Consequences <!-- optional -->

* A known language
* Quick to add new logic
* Easy to document
* Common Layer
* Allows us to mature our architecture

### Negative Consequences <!-- optional -->

* Ruby only
* Can be slow

## Links <!-- optional -->

* [Core Ruby](https://github.com/boodah-consulting/n-vyro-core-ruby) - Core
* [ADR-0008](0008-data-layer.md) - Data Layer - [accepted]
* [ADR-0009](0009-api-layer.md) - API Layer - [accepted]

<!-- markdownlint-disable-file MD013 -->