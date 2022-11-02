# 12 - UI Loading Process

* Status: accepted <!-- optional -->
* Deciders: Yomi (baphled) Colledge <!-- optional -->
* Date: 01/11/2022, 15:01:12 <!-- optional -->
* Template used: [MADR 3.0.0](https://adr.github.io/madr/) <!-- optional -->

Technical Story: text <!-- optional -->

## Context and Problem Statement

We already have the functionality in place for handling our loading process,
however we've neglected to document how this should be implemented and the
reasoning behind it.

When a user visits the devices page, we go through a number of steps to make
sure that the device is not only in the correct state, but also that all data
associated to it is correctly loaded.

For this we introduced the concept of a `SplashScreen` component which allows
us to display a loading page, whilst the application goes through these
motions. We already have the core functionality to make sure that a user never
sees the page whilst it is being loaded and also makes sure that this loading
page is rendered.

However, it is still not particularly clear how client packages should use this
feature. We have kept an example of this logic within
`@n-vyro/bootstrap-frontend` however it has not formally been decided on.

## Decision Drivers <!-- optional -->

* Need to load custom data (based off of device)
* Must be easy to replicate
* Reuses existing components
* Reuses existing logic

## Considered Options

* Mixins
* Hooks
* Shared functionality
* Custom initialisation

## Decision Outcome

Chosen option: "Mixins" and "Hooks", because we want the core process to be
consistent, but we also need users to be able to include all the pieces of data
they need to be loaded before the main UI component is rendered.

We've always had a mix of "Shared functionality" and "Custom initialisation",
this has served their purpose, but it is as lot more intuitive to leverage
"Mixins" and "Hooks".

This allows users to easily reuse the `SplashScreen` component, whilst not
forcing them to only load pre-defined data. We could further improve upon
this, however it is enough to standardise where this logic should live, and
what packages are responsible for which pieces of the process.

Each client package will have a `App` component, it is this component which
will handle the bootstrap process of our applications. This will rely heavily
on the instructions from the `@n-vyro/bootstrap-frontend`s
[README](https://github.com/boodah-consulting/n-vyro-bootstrap-frontend/blob/main/README.md#integration).
This outlines how to integrate the bootstrap process into a client application.

We also need to make sure that, no matter what page a user starts from, the
splash screen will always be rendered, and that all the necessary data is
loaded before the user is able to view the application.

Typically, a user of these components will needs to make some
[API](0009-api-layer.md) calls, and intialise some [data
layer](0008-data-layer.md), for this we have simplified this process. So that
all a user have to do is define the functionality they want, using the hooks
provided by `@n-vyro/bootstrap` and copy a few snippets to fully integrate the
bootstrap process.

This gives those who want to reuse this process an easy way of integrating into
n-vyro.io's [UI](0004-ui-stack.md) loading process.

### Positive Consequences <!-- optional -->

* `App` component becomes the place we initialise the application
* Hooks for plugging into the process
* Small amount of code to get started
* `SplashScreen` handles displaying the splash screen
* `@n-vyro/bootstrap-frontend` becomes the central point of truth

### Negative Consequences <!-- optional -->

* Changing the process within `@n-vyro/bootstrap-frontend` means that each client package will also need to update

## Links <!-- optional -->

* [SplashScreen Setup](https://github.com/boodah-consulting/n-vyro-bootstrap-frontend/blob/main/README.md#integration)

<!-- markdownlint-disable-file MD013 -->