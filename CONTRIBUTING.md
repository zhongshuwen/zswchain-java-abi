# Contributing to EOSIO SDK for Java: ABIEOS Serialization Provider

Interested in contributing? That's awesome! Here are some guidelines to get started quickly and easily:

- [Reporting An Issue](#reporting-an-issue)
  - [Bug Reports](#bug-reports)
  - [Feature Requests](#feature-requests)
  - [Change Requests](#change-requests)
- [Working on ABIEOS Serialization Provider](#working-on-abieos-serialization-provider)
  - [Feature Branches](#feature-branches)
  - [Developing With Gradle Locally](#developing-with-gradle-locally)
  - [Building With Docker](#building-with-docker)
  - [Submitting Pull Requests](#submitting-pull-requests)
  - [Testing and Quality Assurance](#testing-and-quality-assurance)
  - [Code Style and Linting](#code-style-and-linting)
- [Conduct](#conduct)
- [Contributor License & Acknowledgments](#contributor-license--acknowledgments)
- [References](#references)

## Reporting An Issue

If you're about to raise an issue because you think you've found a problem with ABIEOS Serialization Provider, or you'd like to make a request for a new feature in the codebase, or any other reason… please read this first.

The GitHub issue tracker is the preferred channel for [bug reports](#bug-reports), [feature requests](#feature-requests), and [submitting pull requests](#submitting-pull-requests), but please respect the following restrictions:

* Please **search for existing issues**. Help us keep duplicate issues to a minimum by checking to see if someone has already reported your problem or requested your idea.

* Please **be civil**. Keep the discussion on topic and respect the opinions of others. See also our [Contributor Code of Conduct](#conduct).

### Bug Reports

A bug is a _demonstrable problem_ that is caused by the code in the repository. Good bug reports are extremely helpful - thank you!

Guidelines for bug reports:

1. **Use the GitHub issue search** &mdash; check if the issue has already been
   reported.

1. **Check if the issue has been fixed** &mdash; look for [closed issues in the
   current milestone](/../../issues?q=is%3Aissue+is%3Aclosed) or try to reproduce it
   using the latest `develop` branch.

A good bug report shouldn't leave others needing to chase you down for more information. Be sure to include the details of your environment and relevant tests that demonstrate the failure.

[Report a bug](/../../issues/new?title=Bug%3A)

### Feature Requests

Feature requests are welcome. Before you submit one be sure to have:

1. **Use the GitHub search** and check the feature hasn't already been requested.
1. Take a moment to think about whether your idea fits with the scope and aims of the project.
1. Remember, it's up to *you* to make a strong case to convince the project's leaders of the merits of this feature. Please provide as much detail and context as possible, this means explaining the use case and why it is likely to be common.

### Change Requests

Change requests cover both architectural and functional changes to how ABIEOS Serialization Provider works. If you have an idea for a new or different dependency, a refactor, or an improvement to a feature, etc - please be sure to:

1. **Use the GitHub search** and check someone else didn't get there first
1. Take a moment to think about the best way to make a case for, and explain what you're thinking. Are you sure this shouldn't really be a [bug report](#bug-reports) or a [feature request](#feature-requests)?  Is it really one idea or is it many? What's the context? What problem are you solving? Why is what you are suggesting better than what's already there?

## Working on ABIEOS Serialization Provider

Code contributions are welcome and encouraged! If you are looking for a good place to start, check out the [good first issue](/../../labels/good%20first%20issue) label in GitHub issues.

Also, please follow these guidelines when submitting code:

### Feature Branches

To get it out of the way:

- **[develop](/../../tree/develop)** is the development branch. All work on the next release happens here so you should generally branch off `develop`. Do **NOT** use this branch for a production site.
- **[master](/../../tree/master)** contains the latest release of ABIEOS Serialization Provider. This branch may be used in production. Do **NOT** use this branch to work on ABIEOS Serialization Provider's source.

### Developing With Gradle Locally

By default, libraries are installed from remote Maven repositories through Gradle. If, however, you wish to develop locally and you'd like to integrate with locally-cloned versions of EOSIO SDK for Java and/or other Providers, follow these instructions:

1. Clone this and other repos into the same directory, as siblings of one another.
1. Github repo of all libraries:
   * [EOSIO SDK for Java](https://github.com/EOSIO/eosio-java): The core EOSIO SDK for Java library
   * [RPC Provider](https://github.com/EOSIO/eosio-java-rpc-provider): The RPC provider implementation in the core library
   * [ABIEOS Serialization Provider](https://github.com/EOSIO/eosio-java-abieos-serialization-provider): A pluggable serialization provider for EOSIO SDK for Java using ABIEOS (for transaction and action conversion between JSON and binary data representations)
   * [Softkey Signature Provider](https://github.com/EOSIO/eosio-java-softkey-signature-provider): An example pluggable signature provider for EOSIO SDK for Java for signing transactions using in-memory keys (not for production use)1. Import as a gradle project into your favorite IDE or build with gradle from the command line.
1. Develop!

### Building With Docker
Since [ABIEOS Serialization Provider](https://github.com/EOSIO/eosio-java-abieos-serialization-provider) contains a native library component, it is necessary to build it specifically for the target platform it is to be run on. Docker is a good method for doing this.

The repository currently contains two Dockerfiles to use as examples. The default `Dockerfile` is for [Alpine Linux](https://alpinelinux.org/). `Dockerfile.ubuntu` is also provided to build for [Ubuntu 18.04](https://releases.ubuntu.com/18.04/). Other build images can also be created. If you are making an image, be sure the C++ compiler toolchain supports the GNU g++ extensions or equivalent or you will receive build time errors for the native library.

To use the default `Dockerfile` to perform a build, you must first build the Docker image. This can be done from the top level of the checked-out repository with the command:

```
docker build --tag eosio-java-abieos:build .
```

If the Docker image builds successfully, then you should be able to build and jar [ABIEOS Serialization Provider](https://github.com/EOSIO/eosio-java-abieos-serialization-provider) by running the command:

```
docker run --rm -u gradle -v "$PWD":/home/gradle/project -w /home/gradle/project eosio-java-abieos:build ./gradlew --console=rich clean build
```

Any other valid gradle task can be substituted for `jar` in the command above. The Docker images attach the project directory on the host machine, so changes made by the image will be visible to the host machine, and the reverse is also true.

Validate the jar has the correct contents by running the following command, and making sure the output matches what's below. If there is a '.dylib' file(s) or anything else, it might a crash:

```
jar tf build/libs/eosio-java-abieos-serialization-provider.jar
```

Should match:

```
META-INF/
META-INF/MANIFEST.MF
one/
one/block/
one/block/zswjavaabieosserializationprovider/
one/block/zswjavaabieosserializationprovider/JsonUtils.class
one/block/zswjavaabieosserializationprovider/EmbeddedLibraryTools.class
one/block/zswjavaabieosserializationprovider/AbiZswSerializationProviderImpl.class
one/block/zswjavaabieosserializationprovider/AbieosContextNullError.class
one/block/zswjavaabieosserializationprovider/AbiEosJson.class
zswjavaabieos/
zswjavaabieos/build/
zswjavaabieos/build/lib/
zswjavaabieos/build/lib/main/
zswjavaabieos/build/lib/main/debug/
zswjavaabieos/build/lib/main/debug/libzswjavaabieos.so
```

### Submitting Pull Requests

Pull requests are awesome. If you're looking to raise a PR for something which doesn't have an open issue, please think carefully about [raising an issue](#reporting-an-issue) which your PR can close, especially if you're fixing a bug. This makes it more likely that there will be enough information available for your PR to be properly tested and merged.

### Testing and Quality Assurance

Never underestimate just how useful quality assurance is. If you're looking to get involved with the code base and don't know where to start, checking out and testing a pull request is one of the most useful things you could do.

Essentially, [check out the latest develop branch](#working-on-abieos-serialization-provider), take it for a spin, and if you find anything odd, please follow the [bug report guidelines](#bug-reports) and let us know!

### Code Style and Linting

ABIEOS Serialization Provider leverages [SonarLint](https://www.sonarlint.org/) for linting and the [Google Java Style Guide](https://github.com/google/styleguide) with tab size and indent set to 4, and continuation indent set to 8 for code format flagging. Once SonarLint is installed, linting warnings and errors will be flagged inline with squiggles. Automatic code formatting can be accomplished by downloading and importing the Google Java Style settings into your IDE.

Please be sure to resolve any linting issues introduced by your contributions prior to requesting a review on your PR.

## Conduct

While contributing, please be respectful and constructive, so that participation in our project is a positive experience for everyone.

Examples of behavior that contributes to creating a positive environment include:
- Using welcoming and inclusive language
- Being respectful of differing viewpoints and experiences
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

Examples of unacceptable behavior include:
- The use of sexualized language or imagery and unwelcome sexual attention or advances
- Trolling, insulting/derogatory comments, and personal or political attacks
- Public or private harassment
- Publishing others’ private information, such as a physical or electronic address, without explicit permission
- Other conduct which could reasonably be considered inappropriate in a professional setting

## Contributor License & Acknowledgments

Whenever you make a contribution to this project, you license your contribution under the same terms as set out in LICENSE, and you represent and warrant that you have the right to license your contribution under those terms.  Whenever you make a contribution to this project, you also certify in the terms of the Developer’s Certificate of Origin set out below:

```
Developer Certificate of Origin
Version 1.1

Copyright (C) 2004, 2006 The Linux Foundation and its contributors.
1 Letterman Drive
Suite D4700
San Francisco, CA, 94129

Everyone is permitted to copy and distribute verbatim copies of this
license document, but changing it is not allowed.


Developer's Certificate of Origin 1.1

By making a contribution to this project, I certify that:

(a) The contribution was created in whole or in part by me and I
    have the right to submit it under the open source license
    indicated in the file; or

(b) The contribution is based upon previous work that, to the best
    of my knowledge, is covered under an appropriate open source
    license and I have the right under that license to submit that
    work with modifications, whether created in whole or in part
    by me, under the same open source license (unless I am
    permitted to submit under a different license), as indicated
    in the file; or

(c) The contribution was provided directly to me by some other
    person who certified (a), (b) or (c) and I have not modified
    it.

(d) I understand and agree that this project and the contribution
    are public and that a record of the contribution (including all
    personal information I submit with it, including my sign-off) is
    maintained indefinitely and may be redistributed consistent with
    this project or the open source license(s) involved.
```

## References

* Overall CONTRIB adapted from https://github.com/mathjax/MathJax/blob/master/CONTRIBUTING.md
* Conduct section adapted from the Contributor Covenant, version 1.4, available at https://www.contributor-covenant.org/version/1/4/code-of-conduct.html
