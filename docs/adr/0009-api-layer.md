# 9 - API Layer

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 16/10/2022, 01:03:09 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

As we have a [Data Layer](0008-data-layer.md), which handles storing any
incoming data, we need a way of allowing our [UI Stack](0004-ui-stack.md) to
retrieve this data. As we have already established that any service can
communicate with the [Data Layer](0008-data-layer.md) we can then reason that
an API is a service, which can interact with this layer.

However, we need to establish the purpose of our APIs, and when we should use
them. We should also determine how we should implement these APIs.

Our APIs will only deal with historical data, or the computation there of, by
which we mean only data which has been stored within our [Data
Layer](0008-data-layer.md).

API calls are only used to provide us with a way of retrieving all of this
data, based off of the resource and parameters passed. Like any other service,
the API communicates with our [Data Layer](0008-data-layer.md), but it does not
communicate with our [Messaging Layer](0001-messaging-layer.md). This way the
API service is effectively an abstraction layer above our [Data
Layer](0008-data-layer.md).

We already have a [Data Layer](0008-data-layer.md), which we can use for
reading, and writing our data to, but when users are interacting with the UI we
want to also be able to allow them to make calls to our API services. This will
need to be Restful, and return JSON, we also want this versioned, not just on
release, but also the API resources their self. This will allow us to work on
specific pieces of work, without introducing too much complexity to existing
resources.

As we've already got a lot of code written in Ruby, we should _ideally_ look at
a Ruby based solution here, but that shouldn't be the main driver. We also need
to consider how easy it will be to maintain and document our resources.

## Decision Drivers <!-- optional -->

* Quick to build
* Well documented
* Can speak Ruby
* Easy to pick up
* Interacts with [Data Layer](0008-data-layer.md)

## Considered Options

* Rails API
* Sinatra
* Grape

## Decision Outcome

Chosen option: "Sinatra" and "Grape", because we've got a lot of experience
with using these tools. We also know that we can integrate grape-swaggger,
which will be very helpful in documenting our API resources, as we move
forward.

Adding Rails seems like complete overkill, we only need a basic service here,
but we want it to be flexible, and this way we can incrementally enhance this
API without having to deal with all of the noise that Rails can introduce.

We've been using grape for years, so we're pretty comfortable with it, not to
mention that Sinatra is closer to the metal, so we have a lot of control of the
service.

We will also make use of grape-entity, which we will use to define our resource
entities. Again, this can be integrated with grape-swagger, helping us bake in
our documentation.

We had originally had the core of our API written in C/C++, so we've seen how
clunky that can be. With Ruby, we're able to focus on the core requirements
and not have to fuss with anything out side of that scope.

We will also need API versioning, this means we can have individual versions
for both our entities and our API resources. This will help us to support older
systems, whilst still providing new functionality to users with the latest
changes.

As we've already got the [building
blocks](https://github.com/boodah-consulting/n-vyro-core-ruby) for interacting
with [n-vyro.io](https://n-vyro.io) in Ruby, we can take advantage of this code
and be able to reuse code that's been working for a while. This means that our
API service only needs to define the API resources, entities, and
documentation.

### Positive Consequences <!-- optional -->

* A lot of people know Ruby
* Is flexible
* Able to retrieve historical data
* Scalable

### Negative Consequences <!-- optional -->

* Can be slow
* Another service to provision

## Links <!-- optional -->

* [ADR-0001](0001-messaging-layer.md) - Messaging Layer
* [ADR-0004](0004-ui-stack.md) - UI Stack
* [ADR-0008](0008-data-layer.md) - Data Layer

<!-- markdownlint-disable-file MD013 -->