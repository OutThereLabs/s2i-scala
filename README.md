# Source To Image Scala

This enables source to image for Scala applications.

## Installation

To install s2i Scala run:

```shell
$ oc process \
    -f https://raw.githubusercontent.com/OutThereLabs/s2i-scala/master/s2i-scala-template.yaml \
    | oc create -f -
```
