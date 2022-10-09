# 4 - UI Stack

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 09/10/2022, 03:26:15 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

One of the most important aspects of our platform is the UI.

Our UIs need to be as consistent as possible, as well as needing to be easy to
develop with.

Equally to the Firmware Stack, we need to have a standardised way of generating
the required assets. This stack will be used to generate _all_ UI components,
as well as handling user journeys.

When we create a new user journey we will need to be able to easily, and
consistently, document it. We're against too much documentation upfront,
especially when it comes to actually implementing something, however we do
thing that it is _vital_ to add enough documentation to express intent.

This means that we need a way of documenting the intent of our code, whilst
backing up with tests.

It's going to be important that we decide on a stack that is as accessible to
as many developers as possible. This will be important if we want people to be
able to easily jump in and extend the codebase.

We will also want to pay attention to the CI aspect of these builds, lending
from the existing setup existing within the ESP based projects. It will be
important that we're able to do similar here, and that these steps are
replicable in similar types of projects.

Doing this will help to inform us on what we need when it comes to formalising
our CI process.

## Decision Drivers <!-- optional -->

* Easy to test
* Re-useable
* Composability
* Can do e2e, and integration testing
* Can build shareable components/libraries
* Easy to learn
* Can generate mobile applications
* Web-based
* Little context switching

## Considered Options

* ReactJS
* VueJS
* Vuex
* Quasar
* Cypress
* Storybook
* Jest

## Decision Outcome

Chosen option: "VueJS", "Vuex", "Quasar", "Cypress", "Storybook", and "Jest".

We're aware that ReactJS is the most popular library, but we've never taken to
the library. Mostly, there are a million ways to do one things, and we have a
_strong_ preference towards simple, and standardised.

As we've already worked with VueJS, our PoC UI was created using it, we found
that it is _much_ closer to what we're comfortable with.

Shared state is _super_ hard to work with.

Another reason behind choosing "VueJS" is that it comes with a nice library
that handles application state in a really nice way. "Vuex" is our choice here,
it's really easy to test, and it is of a modular design. We're able to easily
test each Vuex module in isolation, and then write clear tests for confirming
their payload, and interaction.

We can also develop these modules away from the our UIs, meaning that we can
re-use them. Another quality we strongly desire.

This covers the core JS library, but we still need to be able to generate
mobile applications from our components. We'll be using "Quasar" to here, it'll
help us with _most_ of the commonly needed components.

"Quasar" is a rich "VueJS" components library, it also has an exceptionally
rich ecosystem; allowing us to generate our mobile applications.

It isn't that common, AFAIK, but it is really simple to use so we'll add it to
our toolbox.

When it comes to testing _core_ user journeys we will use Cypress to help us to
tie things together. This will use "Cucumber". We've decided to use the
"Cucumber" syntax as it is something that _everyone_ can understand. We've had
a complicated relationship with "Cucumber", however, as we are only using it
for _vital_ parts of our UIs, there's not much of an negative impact on our CI.

We'll keep our complex interactions to a minimum here, that way we can keep our
build times as low as possible. For parts of our UI that are a bit more
involved, we have "Storybook" for handling that.

As we're only using "Cypress" in a limited way, we'll keep it in our toolbox,
and review it when things grow.

We'll also "Storybook", it's been a _really_ nice addition to our development
cycle, so it only makes sense to standardised it here. It is a component driven
tool allowing for short feedback loops, via the browser. This is nice when
we're mid failing e2e test, and we want to confirm our components are setup
correctly.

As it lends well to helping us to flesh out our use cases, we use it
extensively for documenting the intent of our UI components.

As it can help us to build up our components, from the ground up, it also
allows us to write stories based off the scenarios we are currently working on.
This helps us to think from the users perspective, it also makes it really easy
to share these components with our users.

As our users are central to our products, we want to know, as soon as possible,
whether we're actually improving our products.

"Storybook" gives us that ability, by allowing us to show off our components so
that they can play with them when we're ready.

It also lends itself well to our common testing practices. Story data is
re-useable. So we can re-use the test data we've already created and use it
within our specs.

Speaking of which, we prefer to develop our code using BDD.

This means that we want to describe the behaviour of things, rather than how a
thing works directly.

This is why we've decided to go with "Jest".

"Jest" allows us to write specs in a very similar way to how we would if we
were writing Ruby, this means that we have less context switching to do.
Leading to better productivity. It is also _really_ well known, so that is an
added bonus.

We use "Jest" extensively within our UI codebase, its core utility is helping
us to describe the intent of our components, objects, and functions. We attempt
to have as high a code coverage as possible, but more important is that we
maintain a high standard when it comes to having well written tests.

We not only have to clear describe the intent, we also need to make sure that
we are testing the write things.

We want our code as expressive as possible, and this means having plan tests,
that can be understood by even novices. Smart isn't always great, we'd rather
simple, plan, and easy to understand.

"Jest" helps us to achieve this.

### Positive Consequences <!-- optional -->

* Easy to extend
* Quasar gives us mobile app generator
* Able to generate static files
* Component based development
* Jest gives us RSpec like testing
* VueX is easy to test

### Negative Consequences <!-- optional -->

* A lot of parts
* Inexperienced people _may_ find it complex
* Using older version of VueJS, Vuex, and Quasar
* Tests _could_ end up being slow

<!-- markdownlint-disable-file MD013 -->