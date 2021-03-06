[up](../../../../GRMustache)

# Forking GRMustache

After you have forked groue/GRMustache, you might want to change stuff, test, and then build the library.

You'll find below some useful information on each of those topics.

## Change GRMustache

Objective-C files that make GRMustache are stored in the `src/classes` folder. They are added to both `GRMustache3-MacOS` and `GRMustache3-iOS` targets of the `src/GRMustache.xcodeproj` project.

Headers are splitted in two categories:

- public headers
- private headers

### Public headers

Public headers must contain only declarations for APIs that are exposed to the GRMustache users. They must not import or include any private header.

Methods and functions declared in public headers must be decorated with the macros defined in `Classes/GRMustacheAvailabilityMacros.h`. Check existing public headers for inspiration.

`src/classes/GRMustacheAvailabilityMacros.h` is generated by `src/bin/buildGRMustacheAvailabilityMacros`.

### Private headers

Private headers have names ending in `_private.h`. They must not import or include any public header. The set of public APIs must be duplicated in both public and private headers.

## Test GRMustache

Before running the tests, make sure git submodules are downloaded:

    $ git submodule update --init

There are two kinds of tests, all stored in the `src/tests` folder.

- tests of private APIs
- tests of public APIs

When a file is added or removed from the `src/tests` folder, both `GRMustache3-MacOSTests` and `GRMustache3-iOSTests` targets of the `src/GRMustache.xcodeproj` project are updated.

### Tests of private APIs

Tests of private internals are stored in the `src/tests/Private` folder, and are all subclasses of `GRMustachePrivateAPITest`.

The implementation files of those tests must not include any public header.

### Tests of public APIS

Tests of public GRMustache API are versionned: the `src/tests/Public/v2.0` folder contains tests for features introduced in the version 2.0 of the library. `src/tests/Public/v2.1` contains tests for the version 2.1, etc.

Those tests are all subclasses of `GRMustachePublicAPITest`. Their implementation files must not include any private header.

You will use the macros defined in `Classes/GRMustacheAvailabilityMacros.h`. They help the tests acheiving three goals:

- use only APIs that are available in the GRMustache version they test against,
- emit deprecation warning when they use deprecated GRMustache APIs,
- help GRMustache achieve full backward compatibility.

For instance, all header files for public API tests in `src/tests/Public/v2.1` would begin with:

    #define GRMUSTACHE_VERSION_MAX_ALLOWED GRMUSTACHE_VERSION_2_1
    #import "GRMustachePublicAPITest.h"

When you add a test for a public API, make sure you place it in the folder that introduced the API (check the release notes), and NOT in the version that will include the new code. For instance, if version 2.6 introduces a fix for an API that was introduced in version 2.2, the version 2.6 will then ship with new tests in the src/tests/Public/v2.2 folder.

## Building

Building GRMustache is building the `/lib` and `/include` folders, which contain public headers and static libraries for iOS and MacOS.

In order to build them: make sure git submodules are downloaded first:

    $ git submodule update --init

Then, issue the following command:

    $ make clean && make

[up](../../../../GRMustache)
