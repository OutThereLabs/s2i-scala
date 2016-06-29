
# scala
FROM openshift/base-centos7

# TODO: Put the maintainer name in the image metadata
# MAINTAINER Patrick Tescher <patrick@outtherelabs.com>

# TODO: Rename the builder environment variable to inform users about application you provide them
# ENV BUILDER_VERSION 1.0

# TODO: Set labels used in OpenShift to describe the builder image
LABEL io.k8s.description="Platform for and running Scala applications in Java 8" \
     io.k8s.display-name="Scala with Java 8" \
     io.openshift.expose-services="9000:http,8778:jolokia" \
     io.openshift.tags="builder,scala,java8"

# TODO: Install required packages here:
RUN yum update -y
RUN yum install -y java-1.8.0-openjdk
RUN curl https://bintray.com/sbt/rpm/rpm | tee /etc/yum.repos.d/bintray-sbt-rpm.repo
RUN yum install -y sbt

# TODO (optional): Copy the builder files into /opt/app-root
# COPY ./<builder_folder>/ /opt/app-root/

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./.s2i/bin/ /usr/libexec/s2i

# TODO: Drop the root user and make the content of /opt/app-root owned by user 1001
RUN chown -R 1001:1001 /opt/app-root

# This default user is created in the openshift/base-centos7 image
USER 1001

# TODO: Set the default port for applications built using this image
EXPOSE 9000
EXPOSE 8778

# TODO: Set the default CMD for the image
CMD ["usage"]
