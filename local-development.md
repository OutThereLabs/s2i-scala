# Local Scala Development with OpenShift

## Install Docker
Install Docker for Mac or Windows from here: https://www.docker.com/products/docker
Then add 172.30.0.0/16 to your insecure registries in using the Docker menu bar item

## Install the OpenShift CLI tools
```bash
brew update
brew install openshift-cli
```

## Set up our cluster
```bash
oc cluster up
oc login -u system:admin
oc patch scc restricted -p '{"allowHostDirVolumePlugin": true}'
oc login -u developer
```

## Set up our application
```bash
oc new-app -f https://raw.githubusercontent.com/OutThereLabs/s2i-scala/master/local-development.yaml --param="SOURCE_PATH=${PWD}"
```
