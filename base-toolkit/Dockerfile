FROM gcr.io/kaniko-project/executor:debug AS kaniko
FROM busybox:1.31.0 AS busybox
FROM jenkins/inbound-agent:alpine

ARG BUILD_DATE
ARG VCS_REF
ARG VCS_URL
ARG VERSION

LABEL maintainer="Marcos Oliveira Soares <marcosoliveirasoares94@gmail.com>" \
  org.label-schema.build-date=$BUILD_DATE \
  org.label-schema.vcs-ref=$VCS_REF \
  org.label-schema.vcs-url=$VCS_URL \
  org.label-schema.version=$VERSION

# By default, the JNLP3-connect protocol is disabled due to known stability
# and scalability issues. You can enable this protocol using the
# JNLP_PROTOCOL_OPTS environment variable:
#
# JNLP_PROTOCOL_OPTS=-Dorg.jenkinsci.remoting.engine.JnlpProtocol3.disabled=false
#
# The JNLP3-connect protocol should be enabled on the Master instance as well.
ENV JNLP_PROTOCOL_OPTS=-Dorg.jenkinsci.remoting.engine.JnlpProtocol3.disabled=false \
      # Disable the JVM PerfDataFile feature by adding `-XX:-UsePerfData` to the
      # `JAVA_OPTS` environment variable. Otherwise, a superfluous
      # `/tmp/hsperfdata_root` directory will be included in the final Docker image.
      JAVA_OPTS=-XX:-UsePerfData

# apk and kaniko must be run as root.
USER root

# Install minimally required packages
RUN apk add --update \
      build-base \
      make \
      # Install additional packages
      jq \
      wget \
      curl \
      python3 \
      libxml2-utils \
      # Install envsubst command for replacing config files in system startup
      # - it needs libintl package
      # - only weights 100KB combined with it's libraries
      gettext \
      libintl && \
      mv /usr/bin/envsubst /usr/local/bin/envsubst && \
      # install the JQ
      #wget https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 && \
      #mv jq-linux64 /usr/local/bin/jq && \
      #chmod +x /usr/local/bin/jq && \
      # install the AWS CLI using the bundled installer
      curl 'https://s3.amazonaws.com/aws-cli/awscli-bundle.zip' -o 'awscli-bundle.zip' && \
      unzip awscli-bundle.zip && \
      python3 ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws && \
      rm -rf awscli-bundle*

ENV SONAR_SCANNER_VERSION=4.7.0.2747
RUN \
    curl -fSL https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-${SONAR_SCANNER_VERSION}.zip -o /opt/sonar-scanner.zip && \
    unzip /opt/sonar-scanner.zip -d /opt && \
    rm /opt/sonar-scanner.zip && \
    sed -i 's/use_embedded_jre=true/use_embedded_jre=false/g' /opt/sonar-scanner-${SONAR_SCANNER_VERSION}/bin/sonar-scanner && \
    ln -s /opt/sonar-scanner-${SONAR_SCANNER_VERSION}/bin/sonar-scanner /usr/bin/sonar-scanner

# The /kaniko directory is whitelisted by default. Its contents are not de-
# leted between stages, nor is it included in the final Docker image.
COPY --from=kaniko /kaniko /kaniko
COPY --from=busybox /bin /busybox

ENV PATH=$PATH:/usr/local/bin:/kaniko:/busybox

ENV AGENT_WORKDIR=/home/jenkins/agent \
      DOCKER_CONFIG=/kaniko/.docker

WORKDIR /home/jenkins/