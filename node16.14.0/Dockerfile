FROM node:16.14.0-alpine AS node
FROM marcosoliveirasoares94/jenkins-inbound-agent:base-toolkit AS base

COPY --from=node /usr/lib /usr/lib
COPY --from=node /usr/local/lib /usr/local/lib
COPY --from=node /usr/local/bin /usr/local/bin
COPY --from=node /usr/local/share /usr/local/share
COPY --from=node /usr/local/include /usr/local/include

RUN apk --no-cache add yarn --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community

LABEL maintainer="Marcos Oliveira Soares <marcosoliveirasoares94@gmail.com>" \
  org.label-schema.build-date=$BUILD_DATE \
  org.label-schema.vcs-ref=$VCS_REF \
  org.label-schema.vcs-url=$VCS_URL \
  org.label-schema.version=$VERSION