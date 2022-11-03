FROM marcosoliveirasoares94/jenkins-inbound-agent:base-toolkit

LABEL maintainer="Marcos Oliveira Soares <marcosoliveirasoares94@gmail.com>"

USER root

ENV JAVA_VERSION=openjdk11
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk
ENV PATH=$PATH:$JAVA_HOME/bin

ENV M2_HOME=/usr/share/java/maven-3
ENV PATH=$PATH:$M2_HOME/bin

# Install additional packages
RUN apk add --update \
    maven \
    openjdk11

LABEL maintainer="Marcos Oliveira Soares <marcosoliveirasoares94@gmail.com>" \
  org.label-schema.build-date=$BUILD_DATE \
  org.label-schema.vcs-ref=$VCS_REF \
  org.label-schema.vcs-url=$VCS_URL \
  org.label-schema.version=$VERSION