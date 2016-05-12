# FFTT: A new concept of build tool. | Massimiliano Mantione @m_a_s_s_i

## Simple is good
Everybody start with few scripts, but then the project grows
Dependency tracking is what is needing.

Recipe for speed:
 - independency (A, B, C, need A, skip B and C)
 - dependency (A > B > C need C skip A, B)

## State of the build
 - Grunt
 - Gulp: pipe
 - Broccoli: file tree instead of pipe
 - Other tools:
  - Webpack: not complete
  - Jake: like make
  - Gradle: java ecosystem
  - Bazel: google

## Whishlist
 - deterministic
 - reliable

How do determine that a build is reliable? You can't

## Dependecy tracking
 - file tree (jake, blake)
 - internally (gulp, Webpack)

## A better way
Track content, not mtime, just how git does. Bazel does it

## An even better way
Store everything in a content adressable file.
Use filesystem as environment to build steps.

Isolate build steps in containers

## THe FFTT choice
Fast, correct

Build system meet functional programming, immutable data structures, memoization of functions result, each step is a pure function.
You never mess with the environment, it's just data transformation other data.

The build graph is declarative and functional. Each build step is imperative but inside a container, not messing up with system.
Programming language independet.
