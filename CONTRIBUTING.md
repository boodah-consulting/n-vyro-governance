# Contributing

Please feel free to submit pull requests or open issues to improve the
functionality of the software.

You should also check the
[issues](https://github.com/boodah-consulting/n-vyro-api/issues) for
the latest discussions involving the current and future versions of the
Contributor Covenant.

## Submitting Issues

Not every contribution comes in the form of code. Submitting,
confirming, and triaging issues is an important task for any project. At
n-vyro.io we use GitHub to track all project issues.

If you are familiar with n-vyro.io and know the component that is causing you
a problem, you can file an issue in the corresponding GitHub project.
All of our Open Source Software can be found in our [n-vyro.io GitHub
organization](https://github.com/n-vyro-io/). All projects include GitHub
issue templates to help gather information needed for a thorough review.

We ask you not to submit security concerns via GitHub. For details on
submitting potential security issues please see
<https://www.n-vyro.io/security/>

In addition to GitHub issues, we also utilize a feedback site that helps
our product team track and rank feature requests. If you have a feature
request, this is an excellent place to start
<https://www.n-vyro.io/feedback/>

## Contribution Process

* Check out the latest main to make sure the feature hasn't been implemented or the bug hasn't been fixed yet.
* Check out the issue tracker to make sure someone already hasn't requested it and/or contributed it.
* Fork the project.
* Start a feature/bugfix branch.
* Commit and push until you are happy with your contribution.
* Make sure to add tests for it. This is important so I don't break it in a future version unintentionally.
* Please try not to mess with the Rakefile, version, or history. If you want to have your own version, or is otherwise
  necessary, that is fine, but please isolate to its own commit so I can cherry-pick around it.


### Pull Request Requirements

n-vyro.io Projects are built to last. We strive to ensure high quality
throughout the experience. In order to ensure this, we require that all
pull requests to n-vyro.io projects meet these specifications:

1. **Tests:** To ensure high quality code and protect against future
   regressions, we require all the code in n-vyro.io Projects to have at least
   unit test coverage. We use [RSpec](http://rspec.info/) for testing
2. **Green CI Tests:** We use
   [TravisCI](https://travis-ci.com/github/boodah-consulting/n-vyro-api)
   to test all pull requests. We require these test runs to succeed on
   every pull request before being merged.

### Code Review Process

Code review takes place in GitHub pull requests. See [this
article](https://help.github.com/articles/about-pull-requests/) if
you're not familiar with GitHub Pull Requests.

Once you open a pull request, project maintainers will review your code
and respond to your pull request with any feedback they might have. The
process at this point is as follows:

1. Two or more members of the owners, approvers, or reviewers groups must
   approve your PR.
2. Your change will be merged into the project's `main` branch
3. Our semantic-release bot will automatically increment the version and update
   the project's changelog with your contribution. For projects that
   ship as a package, semantic-release will kick off a build which will publish
   the package to the project's `current` channel.

### Developer Certification of Origin (DCO)

Licensing is very important to open source projects. It helps ensure the
software continues to be available under the terms that the author
desired.

n-vyro.io uses [the GPLv3.0
license](https://github.com/boodah-consulting/n-vyro-api/blob/main/LICENSE.md)
to strike a balance between open contribution and allowing you to use
the software however you would like to.

The license tells you what rights you have that are provided by the
copyright holder. It is important that the contributor fully understands
what rights they are licensing and agrees to them. Sometimes the
copyright holder isn't the contributor, such as when the contributor is
doing work on behalf of a company.

To make a good faith effort to ensure these criteria are met, n-vyro.io
requires the Developer Certificate of Origin (DCO) process to be
followed.

The DCO is an attestation attached to every contribution made by every
developer. In the commit message of the contribution, the developer
simply adds a Signed-off-by statement and thereby agrees to the DCO,
which you can find below or at <http://developercertificate.org/>.

``` Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I have
    the right to submit it under the open source license indicated in
    the file; or

(b) The contribution is based upon previous work that, to the best of my
    knowledge, is covered under an appropriate open source license and I
    have the right under that license to submit that work with
    modifications, whether created in whole or in part by me, under the
    same open source license (unless I am permitted to submit under a
    different license), as Indicated in the file; or

(c) The contribution was provided directly to me by some other person
    who certified (a), (b) or (c) and I have not modified it.

(d) I understand and agree that this project and the contribution are
    public and that a record of the contribution (including all personal
    information I submit with it, including my sign-off) is maintained
    indefinitely and may be redistributed consistent with this project
    or the open source license(s) involved.
```

For more information on the change see the n-vyro.io Blog post [Introducing
Developer Certificate of
Origin](https://blog.chef.io/2016/09/19/introducing-developer-certificate-of-origin/)

#### DCO Sign-Off Methods

The DCO requires a sign-off message in the following format appear on
each commit in the pull request:

`Signed-off-by: Yomi Colledge <baphled@n-vyro.io>`

The DCO text can either be manually added to your commit body, or you
can add either **-s** or **--signoff** to your usual git commit
commands. If you are using the GitHub UI to make a change you can add
the sign-off message directly to the commit message when creating the
pull request. If you forget to add the sign-off you can also amend a
previous commit with the sign-off by running **git commit --amend -s**.
If you've pushed your changes to GitHub already you'll need to force
push your branch after this with **git push -f**.

### n-vyro.io Obvious Fix Policy

Small contributions, such as fixing spelling errors, where the content
is small enough to not be considered intellectual property, can be
submitted without signing the contribution for the DCO.

As a rule of thumb, changes are obvious fixes if they do not introduce
any new functionality or creative thinking. Assuming the change does not
affect functionality, some common obvious fix examples include the
following:

- Spelling / grammar fixes
- Typo correction, white space and formatting changes
- Comment clean up
- Bug fixes that change default return values or error codes stored in
  constants
- Adding logging messages or debugging output
- Changes to 'metadata' files like Gemfile, .gitignore, build scripts,
  etc.
- Moving source files from one directory or package to another

**Whenever you invoke the "obvious fix" rule, please say so in your
commit message:**

```
------------------------------------------------------------------------
commit 370adb3f82d55d912b0cf9c1d1e99b132a8ed3b5 Author: Yomi Colledge
<baphled@n-vyro.io> Date:   Wed Sep 18 11:44:40 2015 -0700

  Fix typo in the README.

  Obvious fix.

------------------------------------------------------------------------
```

## n-vyro.io Community

n-vyro.io is made possible by a strong community of developers and system
administrators. If you have any questions or if you would like to get
involved in the n-vyro.io community you can check out:

- [n-vyro.io Community Slack](https://community-slack.n-vyro.io/)
