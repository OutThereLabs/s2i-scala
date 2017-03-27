# Local Scala Development with OpenShift

## Getting Started

### Install Docker
Install Docker for Mac or Windows from here: https://www.docker.com/products/docker
Then add 172.30.0.0/16 to your insecure registries in using the Docker menu bar item

### Install the OpenShift CLI tools and Minishift
```bash
brew update
brew install socat openshift-cli docker-machine-driver-xhyve
brew tap caskroom/versions
brew cask install minishift-beta
```

### Set up our cluster
```bash
minishift start --memory 4048
oc adm policy add-scc-to-user hostmount-anyuid system:serviceaccount:myproject:default --as=system:admin
```

### Set up our application
```bash
oc new-app -f https://raw.githubusercontent.com/OutThereLabs/s2i-scala/master/local-development.yaml \
  --param="SOURCE_PATH=${PWD}" \
  --param="APPLICATION_NAME=${PWD##*/}" \
  --param="DATABASE_SERVICE_NAME=${PWD##*/}-db"
```

## What's happening here

Normally in OpenShift we create a new Docker image by layering on our code and/or binaries on top of an [S2I](https://github.com/openshift/source-to-image) image. For local development we don't want to compile our application and we don't want to create a new image since that would lock our code at a specific point. Instead we launch a new pod with a volume that points at a local directory (in this case `${PWD}`). This allows us to use frameworks like Play's [auto-reload](https://www.playframework.com/documentation/2.5.x/PlayConsole) which will recompile any source files we modify.

OutThereLabs' s2i-scala uses the `DEV_MODE` env var to specify that we should start using `sbt run` instead of locating a binary in `universal/stage/bin`. This is the standard development mode for the Play framework but you should be able to adopt any sbt application to work just as well.

You can connect a local debugger to port 5005.

```bash
oc port-forward $(oc get pods -l name=${PWD##*/} -o name | sed 's/^.*\///') :5000
```


We can use a similar approach on a remote server using [`oc rsync`](https://docs.openshift.org/latest/dev_guide/copy_files_to_container.html) but we would need to use an image that already has some source code in it otherwise our application wouldn't start.
