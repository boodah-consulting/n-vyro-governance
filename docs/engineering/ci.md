# CI

CI will be triggered any time there is a commit to any of our repositories.

Depending on the kind of change it is, CI will trigger a specific sequences of stages. This happens for a number of
reasons, which we'll get into shortly.

## Build Stages

First we should go over what our CIs core duties are:
  * Run our tests
  * Generate artifacts
  * Tag releases

  * New PR
    - Code Standards
      * [Code Quality](#Code Quality)
      * [html markup](#HTML Markup)
      * [build](#Build Artifacts)

  * New Branch/commit
    - Build stages:
      * [unit](#Unit Tests)

  * Merging PR/Change to a release branch
    - Integration stages:
      * [e2e](#E2E Tests)
      * [build](#Build Artifacts)
      * [Code Quality](#Code Quality)
      * [release](#Release New Version)

  * Tag release
    - Build stages:
      * [unit](#Unit Tests)
      * [assets](#Release Artifacts)

## Code Quality
  * Lint check
  * Other forms of code standardisation
  * Code coverage

## HTML Markup
  * semantically the same
  * no "unexpected visual changes"

## Unit tests
  * Run our corresponding unit tests
  * Generate code coverage

## E2E tests
  * Run our corresponding e2e tests
  * Upload videos of ran tests, to S3
  * Upload screenshots, if tests fails, to S3

## Build

This is handy for when we're internally testing various changes against each other.

With our build scripts we can easily pull these files down, extract them to te appropriate place, and generate our
binaries manually.

Not only will this help with development, in general, as it will allow us to be able to develop against a given
version. It will also give us the added bonus of being able to drill down to the specific change, when it comes to
debugging.

Each of our artifacts will be have a name which corresponds to it's git SHA, this way it should be easily identifable,
if we ever need to search out issues for a given release.

Along with this, we'll also upload screenshots for any of our e2e tests, that are failing.

NOTE: We could later leverage these and publish them to the corresponding PR

### Tasks
  * Generate assets, and update release page with files
  * Upload e2e result vidoes to S3, for review
  * Generate CHECKSUM.md
  * Generate CHANGELOG.md

## Release New Version

### Tasks
  * Tracks changes to branch, since last release
  * Tags the release
  * Updates release version
  * Updates the appropriate distribution channels

  * Updates CHANGELOG
  * Updates documentation
  * Update dates

## Release Artifacts

### Tasks
  * Generates artifacts for this Git SHA/tag.
    - updates the release with the appropriate artifacts
  * Generate CHECKSUM.md
  * Update central point of truth with latest release
    - This will be what ever the user interfaces with
      - In our case, that will be the UI

# Tag a release

### Tasks
  * Create new releases
  * Create mew branch for release, if needed
  * Generate release page
    - Lists changes since last release, in this distribution channel
      * Options:-
        * Slack
        * GraphQL
        * Static webpage
