# Semantic Release

It's important to keep track of the changes that happen within our software, and one of the ways we achieve this is by
managing our changes via semantic version.

We won't go too much into explain this here, as there's a excellent resource for that
[here](https://github.com/semantic-release/semantic-release).

But what we will do is we'll take a walk through not only utilsing semantic versioning as part of our continious
integration pipeline, but the benefits of doing so.

## What we want

We need to be able to cut new release, whilst not having to worry about too much about what steps are required, and
how we should go about doing so. For this we can make use of semantic-release, which with a little bit of love, is
quite powerful, especially once used with a CI solution.

We'll be using TravisCI, as well as a simple JS apllication as an example package to release and deploy.

We'll also be conforming to a commenting standard, this will be the trigger for our releases, as well as help us to
keep our commit as descriptive as possible.

There shouldn't be any direct commiting to the main branch, and we should addopt a PR workflow that allows us to tests
new changes, and other checks we have currently setup.

## The How

To help with most of the hard work, we'll be using
[semantic-release](https://github.com/semantic-release/semantic-release) to to most of the heavy lifting.

Not only will this help us deal with automatic versioning, but it will also help us make the necessary changes needed
when making a release.

There's so much you can do with this package, so it's wise to have a sift through the repository and see what else you
can achieve with it.

For now ... we simply want to set it up for our application, and get TravisCI to run it when ever we make a change to
the main branch.

### Comment standard

  * Must be prefixed with one of the follow:
    * feat
    * fix
    * chore
    * build

## Recipe

* semantic-release
* CI (Continious Integration)
* Generate artifacts
* GH Account (solely for build and CI tasks)
