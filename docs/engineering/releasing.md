# Automatic releases

One of the most painful aspects of development is having to manage releases, not only when do we bump a piece of
software's version, but also how should we release these changes once they've been through the whole development pipeline.

For us, we've leveraged an npm packaged called: `semantic-release`, which allows us to specify how we release new
code, along with the commit convention used to determine whether new changes are a patch, minor, or major change.

Even though this is a npm tool, we can easily use it for all of our repositories, allowing us to have fine grained
control of our release, along with how each repository packages up is code.

Throughout this series we'll take you through the steps of not just setting up `semantic-release` but also demonstrate
how we use it within a varieties of scenarios.

Primarily we use TravisCI, for our CI solution, so we'll be using that in this series, but it's pretty straight
forward to migrate this workflow into your CI of choice.

# Setup
First off, we'll need to make sure we have the packages we need to automate our releases. For this we'll add
`semantic-release` along with a couple of additional packages.

`npm install --save-dev semantic-release @semantic-release/changelog @semantic-release/git @semantic-release/exec`

If you don't already have a `package.json` file, now you will, this will keep track of the current version of each of
the packages, but we can also use it to keep track of the projects version. This is why we've added
`@semantic-release/exec`, this will allow us to run an arbitary command when releasing which gives a quite a bit of
flexibility and control later on down the road.

## Release branches

* No commits to the main release branches
  - these should only be interacted with by CI.

# New feature

* [New PR](New PR)
* [PR Checklist](PR Checklist)
* [PR Review](#PR Review)

## Create new PR

  * In line with the naming conventions of [Conventional Commits](https://conventionalcommits.org) PR titles  must be
    prefixed with one of the follow:
    * feat
    * fix
    * chore
    * build
    * docs

  * Description must clearly outline the core purpose of this change
    - This will be dependent on the type of change being made
      * fix may need a reproducable example?
      * feature must have a relating feature files, which have been approved

  * Optional:
    - reviewer

  * Appropriately tagged
  * Must be against the correct branch
    * `main` to become the latest build
    * `next` to become the next release
    * `1.x.x` to focus on a given major
    * `1.2.x` to focus on a given minor
    * `beta` to focus on a given minor
    * `alpha` to focus on a given minor
    * `prerelease` to focus on a given minor

## PR Checklist

  * All tests pass
    - no pending
  * Pass linting
  * Code Quality
    - Code coverage
  * Include description of new change, and impact
  * Is backwards compatible?

## PR Review

* Change makes sense to the eco-system
* Conforms to the branding style guide
* Appropriate documents (changes?)

## Merge PR

* Triggers release build

## Close PR

## Releases

## CI

# Config
