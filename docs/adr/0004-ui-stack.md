# 4 - UI Stack

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 09/10/2022, 03:26:15 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

One of the most important aspects of our platform is the UI. It is here where
the core of our users will interact with the ecosystem. So it needs to be
consistent, well documented, easy for developers to use, and intuitive to user.

It is also where we standardise the look and feel of
[n-vyro.io](https://n-vyro.io), allowing us to have a consistent feel
throughout our UIs. It will allow users to interact with the ecosystem,
providing a common communication layer between our services, and the user. This
stack will be used to generate _all_ UI components, UX flows, as well as being
where our design system lives.

When it comes to making changes we want small feedback loops, so we want a
stack that lends well to this. We also think that its _vital_ to have a decent
level of documentation to help users to use our components, and libraries. As
this is the layer that users will typically communicate with, we need to
document the intent of the components, and have a way of reusing this
documentation in other mediums.

As there will most definitely be complexity within our components, and
libraries, we will want to have a good level of tests here, doubling up as
documentation, but also helping others to understand how specific parts of the
stack work.

We want a stack that is as accessible to as many developers as possible, whilst
being productive and allowing us focus on expressing our ideas. As we want
people to be able to easily jump in and extend the codebase, we must enforce
some processes to make sure that we can maintain our focus on implementing, and
solving problems. It will also be important to have things like code styling,
and coding standards need to be baked into our processes.

Reviewing code takes a lot out of a person, so we want to automate away as much
of this as possible. We also want to make sure that we are testing the core
aspects of our components, making sure that we are not introducing any
unexpected behaviour, and that our code is consistent through the stack.

This means that we need to pay attention to the CI aspect of these builds,
these must not deviate too far away from the workflow we have in other stacks,
but also allow us to add the necessary steps needed for UI development. This
will become important later, when it comes to formalising our CI process.

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

We're aware that ReactJS is the most popular framework, but we've never taken
to it. Mostly, there are a million ways to do one thing, and we have a _strong_
preference towards simple, and standardised solutions. As we've already worked
with "VueJS", our PoC UI was created using it, we found that it is _much_ closer
to what we're comfortable with.

Shared state is _super_ hard to work with. This is another reason behind
choosing "VueJS". It comes with a nice library for handling application state
in a really nice way. "Vuex" is our choice here, it's really easy to test, and
it is modular design. So we're able to easily test each "Vuex" module in
isolation, and then write clear tests for confirming their payload, and
interactions. Developing these modules separately from the UIs, meaning that we
can re-use them throughout our components.

We'll also be using "Quasar" to here, it'll help us with _most_ of the commonly
needed components. "Quasar" is a rich "VueJS" components library, it also has
an exceptionally rich ecosystem; allowing us to generate our mobile
applications. It isn't that common, AFAIK, but it is really simple to use, so
we'll add it to our toolbox.

When it comes to testing our _core_ user journeys Cypress can help us to tie
our components together. We've decided to use the "Cucumber" syntax as it is
something that _everyone_ can understand, and it is pretty good at describing
scenarios, but these do come at a high cost mostly as we have to create known
state. Knowing this, we only want to focus on testing the _vital_ parts of our
UIs, that way we're only testing functionality that's central to the ecosystem.
Whilst using other methods for focusing on an individual component. As we want
short feedback loops we'll want to keep our complex interactions to a minimum
here, that way we can keep our build times as low as possible. As we're only
using "Cypress" in a limited way, we'll keep it in our toolbox, and review it
when things grow.

For the components we make use of "Storybook". It is a browser based
component-driven tool allowing for short feedback loops. This is interesting as
it allows us to think more about the interaction of the component rather than
how do we get this test to pass. It can also be useful for confirming our
components are set up correctly.

As it can help us to compose our components into even more complex component,
from the ground up, it also allows us to write stories based off the scenarios
we are currently working on. This helps us to think from the users' perspective
making it really easy to share these components with our users.

This is a really useful tool for fleshing out our use cases. We use it
extensively for documenting the intent of our UIs. As our users are central to
our products, we want to know, as soon as possible, that we're actually
improving our products. "Storybook" gives us that ability, by allowing us to
show off our components so that they can play with them when we're ready. It
also lends itself well to our common testing practices. Story data is
re-useable. So we can re-use the test data we've already created and use it
within our specs.

Speaking of which, we prefer to develop our code using BDD. We not only have to
clearly describe the intent of our code, we also need to make sure that we are
testing the write things. This means that we want to describe the behaviour of
things, rather than how a thing works directly. This is why we've decided to go
with "Jest".

"Jest" allows us to write specs in a very similar way to how we would if we
were writing Ruby, this means that we have less context switching to do.
Leading to better productivity. It is also _really_ well known, so that is an
added bonus. We use "Jest" extensively within our UI codebase, its core utility
is helping us to describe the intent of our components, objects, and functions.
We attempt to have as high a code coverage as possible, but more important is
that we maintain a high standard when it comes to having well written tests.

We want our code as expressive as possible, and this means having plain tests,
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
* e2e _could_ end up being slow

## Links <!-- optional -->

* [VueJS](https://v2.vuejs.org)
* [VueX](https://v3.vuex.vuejs.org)
* [Quasar](https://v1.quasar.dev/)
* [ReactJS](https://reactJS.org)
* [Jest](https://jestjs.io)
* [Storybook](https://storybook.js.org)
* [Cypress](https://cypress.io)

<!-- markdownlint-disable-file MD013 -->