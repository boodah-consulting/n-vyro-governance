# Gitflow

* No commits to the main release branches
  - these should only be interacted with by CI.
* All commits are signed
  * Using GPG

# New feature

* [Create New PR](#Create New PR)
* [PR Template](#PR Template)
* [PR Checklist](#PR Checklist)
* [PR Review](#PR Review)

## Create new PR

  * In line with the naming conventions of [semantic-release](https://github.com/semantic-release) PR titles must be
    prefixed with one of the follow:
    * feat
    * fix
    * chore
    * build

  * Description must clearly outline the core purpose of this change
    - This will be dependent on the type of change being made
      * fix may need a reproducable example?
      * feature must have a relating feature files, which have been approved
      * each commit make sense and have a self-explaining message
      * there is no unnecessary commits (such as "typo", "fix", "fix again", "eslint", "eslint again" or merge commits)

  * Optional:
    - reviewer

  * Appropriately tagged
  * Must be against the correct branch
    * `main` to become the latest build
    * `1.x.x` to focus on a given major
    * `1.2.x` to focus on a given minor
    * `beta` to focus on a given minor
    * `alpha` to focus on a given minor
    * `prerelease` to focus on a given minor

## PR Checklist

  * All tests pass
    - no pending
  * Pass linting
  * Include description of new change, and impact
  * Is backwards compatible?

## PR Review

* Change makes sense to the eco-system
* Conforms to the branding style guide
* Appropriate documents (changes?)

## Merge PR

* Triggers release build

## Close PR
