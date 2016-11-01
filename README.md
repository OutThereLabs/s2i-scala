# Source To Image Scala

This enables source to image for Scala applications.

## Installation

To install s2i Scala run:

```shell
$ oc process \
    -f https://raw.githubusercontent.com/OutThereLabs/s2i-scala/master/buildconfig.yaml \
    | oc create -f -
```

You can also use s2i Scala for [local development].(local-development.md)
