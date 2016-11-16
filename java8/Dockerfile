# scala
FROM openshift/base-centos7

MAINTAINER Patrick Tescher <patrick@outtherelabs.com>

ENV BUILDER_VERSION 1.0

# Labels used in OpenShift to describe the builder image
LABEL io.k8s.description="Platform for running Scala applications in Java 8" \
      io.k8s.display-name="Scala with Java 8" \
      io.openshift.expose-services="9000:http,8778:jolokia" \
      io.openshift.tags="builder,scala,java8"

# Install Java and SBT
RUN yum update -y && \
    yum install -y java-1.8.0-openjdk && \
    curl https://bintray.com/sbt/rpm/rpm | tee /etc/yum.repos.d/bintray-sbt-rpm.repo && \
    yum install -y sbt

# Copy test app to cache directory
COPY ./test/test-app /opt/test-app-root

# Cache SBT dependencies and plugins from test app
RUN cd /opt/test-app-root && \
    ./sbt \
    -ivy /opt/app-root/src/.ivy2 \
    -sbt-dir /opt/app-root/src/.sbt \
    -sbt-boot /opt/app-root/src/.sbt/boot \
    compile

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./.s2i/bin/ /usr/libexec/s2i

# TODO: Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chown -R 1001:1001 /opt/app-root

# This default user is created in the openshift/base-centos7 image
USER 1001

# Expose default Play and Jolokia ports
EXPOSE 9000 8778

# TODO: Set the default CMD for the image
CMD ["usage"]
