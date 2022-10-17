# 11 - Common Library

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 16/10/2022, 04:57:55 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

Even though we have a [Common Core](0010-common-core.md) we also need to have a
subset of objects which help us to generate, check, and work with our domain
logic. These will live within the core, but they also aren't _strictly_ part of
the [Common Core](0010-common-core.md).

These will include the following:

* Core
* Config
* Matchers
* Builders
* Generators
* Extractors
* Services

#### Core

This consists of all the classes within our [Common Core](0010-common-core.md),
allowing for the whole library to handle everything central to the workings of
[n-vyro.io](https://n-vyro.io). The core will be the foundations of which the
common library stands upon.

#### Config

There's often configuration based logic that's required throughout our
ecosystem, for this reason we have this namespace. We don't want to think too
much about configuration management. We're not entirely sure whether this will
be generalised throughout the system, but we know that we need it within our
Ruby services.

#### Matchers

A matcher helps with checking a dataset against a set of defined values. Two of
the core matcher we have is for an event, and we also have a device matcher.
Our event matcher allows us to check whether a topic matches an event. Whereby
our device matcher helps us to match a device message against a device. Both
are very common pieces of functionality, and allow us to simplify how we check
against each dataset, they can also get quite complicated. So having a way of
checking against them becomes invaluable.

#### Builders

There are a few places where we need to create a number of defined datasets,
but the data we want is complex. For these occasions we need to be able to
comfortably be able to generate the desired collection without having to know
how to create each entry. Events, and Areas are two of examples of this.

Our builders must be able to create randomly generated a collection, based off
of a users requirements. The amount within the collection can be defined by
users, and must return an object from the [Data Layer](0008-data-layer.md).
This helps others to quickly generated a specified number of randomly generated
data which they can then use for testing out various aspects of the system.

Whenever we create a `NVyro::Builders` we should create an accompanying
generator. It will abstract away all the complexity behind what the
generated data set is, and will give us a boundary for randomising that
dataset.

#### Generators

A generator is used to create a single dataset using our [Data
Layer](0008-data-layer.md). Whenever we want to create a complex piece of
random data. We would use this object for the task.

All generators start with the `NVyro::Generators` namespace. These are
typically used by `NVyro::Builders` for generating the randomised data, but
they _should_ also be flexible enough to take seed data, allowing users to be
able to override the default randomised data, but if that's not the case then
we should use [Faker](https://github.com/faker-ruby/faker) for creating
randomly generated values. We also expect that the resulting object should
conform to our [Data Layer](0008-data-layer.md) specifications. This way we
have a consistent way of creating new generators, and it will make it easier to
compose more complex builders.

#### Extractors

There are a few occasions where we need an extra a value(s) from one or more
objects, and return a [Data Layer](0008-data-layer.md) based object. These are
needed to simplify how we extract data out of a more complex object(s). We want
to make sure that these are required across the whole system, or atleast
important enough to be considered part of the core.

An example of this is Trigger. We need to be able to extract a trigger
value, and then compare that with an incoming message. This requires us to take
an existing trigger, along with an incoming message, and then generate an
output that we can work with.

#### Services

Service objects are for working a specific task, and that task only. We
typically use these as a wrapper around our [Data Layer](0008-data-layer.md),
or the [Messaging Layer](0001-messaging-layer.md), giving us an abstraction
layer between our abstraction layers and something we want to do.

We will have a number of services, but not all of them belong here. If it is
only required for a specific device, then we would place this service close to
that device, otherwise we can be certain that this is not the home for it,
otherwise, this is the ideal place for any services that are device wide. These
classes also start with the namespace of `NVyro::Services`, it should use
dependency inject for complex tasks, and pass the main arguements to a `call`
method. This method will be the primary way of calling a service.

## Decision Drivers <!-- optional -->

* Needs to work with existing code
* Easy to work with
* Able to extend

## Considered Options

* Monorepo
* Separate dependency
* No clear namespace separation

## Decision Outcome

Chosen option: "Monorepo", because there's a close dependency between the core
classes, and our fledgling library. There's no real reason to add another layer
to the system, and this library is useful throughout the services. There could
be a case to extract parts out of this later, similarly to [common
core](0010-common-core.md), we could extract this into a lower level language,
but for now we'd like to keep this logic close to the core, and focus on
maturing the logic.

Our common library will work directly with our [Data
Layer](0008-data-layer.md), meaning that they will take [Data
Layer](0008-data-layer.md) based objects, and also return them. This means that
whenever we're working with this layer we know exactly what type of objects we
will be getting back.

At present this isn't _quite_ the case, but formalising this here, should help
to change this. In the future there will likely be more namespaces added to our
common library, which is totally fine, however we should be clear about what we
consider "common" and what isn't.

A piece of functionality that is used throughout the ecosystem. If the
intent is for this functionality to be shared throughout the ecosystem, then
it should be implemented here.

It must either communicate with the [Common Core](0010-common-core.md), [Data
Layer](0008-data-layer.md), or the [Messaging Layer](0001-messaging-layer.md).
This allows us to create library objects that abstract away the complexity of
the lower level of the system, so a user of the library can focus on what's
important to them, getting stuff done.

### Positive Consequences <!-- optional -->

* Keeps all logic close to the source
* Easily integrates with existing code
* Less context switching

### Negative Consequences <!-- optional -->

* Can be slow
* Must know Ruby

## Links <!-- optional -->

* [Faker](https://github.com/faker-ruby/faker) - Faker
* [Core Ruby](https://github.com/boodah-consulting/n-vyro-core-ruby) - Core
* [ADR-0001](0001-messaging-layer.md) - Messaging Layer - [accepted]
* [ADR-0008](0008-data-layer.md) - Data Layer - [accepted]
* [ADR-0009](0009-api-layer.md) - API Layer - [accepted]
* [ADR-0010](0010-common-core.md) - Common Core - [accepted]

<!-- markdownlint-disable-file MD013 -->