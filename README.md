# Source To Image Scala

This enables source to image for Scala applications.

## Installation

To install s2i Scala run:

```shell
$ oc process \
    -f https://raw.githubusercontent.com/OutThereLabs/s2i-scala/master/buildconfig.yaml \
    | oc create -f -
```

You can also use s2i Scala for [local development](local-development.md).

## Binary Builds

You can do a binary build by attaching the contents of target/universal/stage insteaf of source code:

```shell source to build
sbt stage
s2i build --pull-policy=never --copy target/universal/stage outtherelabs/s2i-scala my-app
```

## Running Tests

To run the tests first install greadlink if you are on a mac

```shell
$ brew install coreutils
```

Then use the MAKEFILE

```shell
$ cd java8 && make test
```
