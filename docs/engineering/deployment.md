# n-vyro.io deployment

We need to be able to automatically version and archive each of our packages, in a way that allows us to generate the
binary files needed to update our ESP8266 devices over OTA.

To achieve this we need to tackle a few obstacles:
* test automation
* generating build artifacts
* version control

## Test Automation

Each package needs to have a suite of unit tests, along with some cucumber tests, so that we can run these within CI.
These should cover the core functionality of each feature, along with any bugs or patches over time.

## Version Control

Version control will be the main trigger for firing of new release builds. We'll be making use of the node semver
package which allows us to automatically tag new releases when ever there's changes made to master.

### Build steps

This means that we'll need to only trigger this when we're merging with master, which should then trigger a travis
job, which runs the tests, generates the build files, and if successful with all of the above, generate our artifacts
and generate a new release.

## Generating Build Artifacts

We'll take advantage of travisCI and S3 storage here. Once we have a successful build, and we have merged to master,
we need to make sure that we build the latest changes, and trigger changes within our firmware repositories.

The purpose of this will be to make a git commit, annotating the changes in the related UIs. This will then checkout
the latest release version (potentially this the token we track to determine a change in the CI) and we then build the
new binaries for the UI.

These binaries should be included in the latest release process, along with tarballs of the corresponding UI buids.

## Build Phases

* PR

On each PR we need to make sure that not only are all tests passing, but that the code is inline with our current
coding standards. Once this has been achieved, the build should also be successful

  - unit tests pass
  - linting pass
  - build successful

* Release

A release happens when we've merged a PR into the `main` branch, and all the tests there have successfully passed.

  - unit tests pass
  - integration tests pass
  - linting pass
  - build successful

Once these steps have succeeded we need to generate our artifacts, and cut a release.

  - generate artifacts from build files
  - use semver script to tag generate new version
  - generate a new GH release
  - update related firmware (NVyoConnect and NVyoSense) with new UI versions

### Side note

We may need to consider how we handle dependabot, which can be quite noisey, creating a lot of automatic bumps. We
want to be able to update often, but we also don't want this to create a constant stream of changes. This could make
it really hard, in the future, to track down issues that may arise.

One way around this would be to use another branch to trigger these builds, this way we'll always have a chance to
internally test out new releases before they enter the wild.


## Release

There are a number of release cycles that we'll need to juggle, but at the same time this will help us to move quickly
and be able to revert changes relatively easily.

You have the following UI interfaces:
  * @n-vyro-hub-ui
    - Used by n-vyro-hub
  * @n-vyro-frontend-ui
    - Used by the following:
      * n-vyro-zone
      * n-vyro-soil
      * n-vyro-control

Then you have the back cores for the following:
  * C/C++
    - n-vyro-sense
    - n-vyro-control

  * Ruby
    - n-vyro-hub
    - n-vyro-hub-api

### PCB firmware

We don't need to generate our UI binaries within the C++ based projects, we can simply import the corresponding
project into the UI assets build stage, so that we can generate it there. This will provide us with greater control on
releasing new UI releases, as well as providing us with a relatively fluid path to generating our binaries in the
correct place. We'll need to confirm that platformio indeed generates builds that aren't directly related to the build
size of the firmware binary.

### Hub releases

Our hub releases are a little bit more involved as there are a few more dependencies to deal with. Firstly we'll need
to bundle up each project as a ruby gem. Once this is complete we'll need to leverage our build repository and create
a provisioning task to allow us to install these gems, along with their dependencies. This will also need to include
things like updating our datastore, and other tasks. Along with being able to restart the server, once the
installation is successful.

To do this, we'll use something like FPM, which can help us to geenerate a Debian package, which we can then attach to
our tagged releases, and distribute to our users.

## Distribution

We'll need to be able to provide these release directly to users, with as little interaction as possible.

As we're still in the early days, it would be wise to be able to have more of a controlled release process. Basically
internally releasing versions to ourselves, whilst we test them in the real world. Once we're confident that the
changes we've made are solid, we can then merge those changes into the `main` branch.

We should also have a `prerelease`, `alpha`, and `beta` branches, which we'll later expose to limited people, as we
grow.

As we don't have the functionality for updating these devices yet, we don't have to worry too much about exposing all
releases to the world. It will be enough just having a basic distribution path setup so we can extend upon it later.

### Initial steps

for the time being we'll investigate leveraging GitHub, and S3, for handling our binaries. It would be nice to expose
these to the user in a way that not only allowed them to upgrade. It may also be wise to generate a checksum file, for
these binaries and the debian package. So that we can have a decent level or security that our files are downloading
properly.