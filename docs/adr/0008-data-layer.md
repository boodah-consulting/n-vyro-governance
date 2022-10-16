# 8 - Data Layer

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 16/10/2022, 00:23:32 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

As we keep our UIs thick, and our services thin, we need a way of retrieving
historical data. We need to have a method of storing this data and being able
to interact with it at our convenience. This data will come in a number of
formats, so being able to clearly define this, but also have enough flexibility
to be able to change these quickly will be important. We will also want this
abstraction to be accessible throughout the core of our services. We already
know that we mostly build our services with Ruby, so it will be a natural step
to develop them using this language. We also need to consider how easy it will
be to migrate to another data store, if we later decide to.

This data will then be used by other services, such as APIs, and daemons.
Meaning that we'll have to be able to make changes in as few a place as
possible. We also need to be able to communicate with our data layer
seamlessly. It is important to differentiate between real-time and historical
data. Real-time data is typically sent our by a device, we don't care which
devices are listening out for the message, but we do care about where the
message is sent. This allows us to have a messaging layer that can quickly send
out messages, but not worry about storing or doing anything with this data. For
historical data we typically store this via an [n-vyro.io](https://n-vyro.io)
service. An [n-vyro.io](https://n-vyro.io) service can communicate with our
messaging layer and our data layer. This means that we will likely have a
number of queries and service calls which are commonly used. Meaning that we
will need to centralise this logic so that it is accessible to all services.

## Decision Drivers <!-- optional -->

* Quick to build
* Well documented
* In Ruby
* Seamless communication
* Separation of concerns
* Easy to pick up

## Considered Options

* MongoDB
* Postgres
* GraphQL

## Decision Outcome

Chosen option: "MongoDB", because we know we can easily use this data store, we
also know that it is supported by frameworks like ROM, which we can later use
to refactor our code. This will be important for when we explore migrating to
Postgres. We're aware that Postgres is likely more performant, but we also know
that it would take more effort to work with. Not only because we've got more
MySQL, and MongoDB experience, but also partly due to our data structures
changing a lot. So we didn't want to have to constantly deal with migration
tools, especially when it came to provisioning.

Choosing MongoDB meant that we had to restrict ourselves to MongoDB v2.4, which
is ancient, but we've now migrated to v4.2. One of the aspects that we aren't
that hot on is indexing MongoDB documents, this is definitely a shortcoming of
this approach. However, there need for this not to large, so we've decided to
not worry too much about this.

We will also make use of Query objects, for making our common queries, this
should make it easier to refactor when we want to introduce ROM. We also want
to make sure that we're not using any `NVyro::Model` classes directly. As these
are solely for communicating with the DB. Ideally we want to communicate
directly with `NVyro::Queries` or `NVyro::Services`. These will encapsulate all
the logic needed to handle a query or computation, and then return appropriate
`NVyro::ValueObject`, or `NVyro::Struct`.

At present `NVyro::ValueObject` classes are more explicitly defined, typically
having to extend a class, and handle its serialisation. `NVyro::Struct` is more
dynamic, and leverages Ruby's `Structs`. There is still a discussion around
which approach is best, however our current approach is to create
`NVyro::ValueObject` when we've got a solid structure, and `NVyro::Struct` when
we have something dynamic which is up for change.

If there isn't an appropriate class for the action that you are attempting to
make then we suggest that you implement your own and create a PR. Equally, if
there is a new data structure, which we need to store, then we should create
the appropriate `NVyro::Model` for it and merge than into the ecosystem. If we
know that this data structure is needed by a service, or query, then we suggest
that those are introduced at the same time, or as soon as possible.

The purpose of this layer is to provide a common interface for all n-vyro.io
based services to make use of, this will not only create a central point of
truth for all of our data structures, but it will provide a consistent
interface for interacting with this data. It will be this layer that all
services communicates with, and regardless of language, we should endeavour to
establish these boundaries as soon as possible. Not only to keep things
consistent, and lower context switching, but to also make it clear what the
intent of our code is.

### Positive Consequences <!-- optional -->

* A lot of people know Ruby
* Is flexible
* Able to retrieve historical data
* Established a common communication layer
* Clearer intent
* Can extract code to C/C++, Rust, Go

### Negative Consequences <!-- optional -->

* Can be slow
* Extra layer above models
* Another layer of abstraction
* Restricts us to Ruby

<!-- markdownlint-disable-file MD013 -->