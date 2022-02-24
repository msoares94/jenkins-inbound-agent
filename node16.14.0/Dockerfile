FROM marcosoliveirasoares94/jenkins-inbound-agent:base-toolkit AS base
FROM node:16.14.0-alpine AS node

FROM base

COPY --from=node /usr/lib /usr/lib
COPY --from=node /usr/local/share /usr/local/share
COPY --from=node /usr/local/lib /usr/local/lib
COPY --from=node /usr/local/include /usr/local/include
COPY --from=node /usr/local/bin /usr/local/bin

LABEL maintainer="Marcos Oliveira Soares <marcosoliveirasoares94@gmail.com>"

USER root

RUN apk --no-cache add yarn --repository=http://dl-cdn.alpinelinux.org/alpine/edge/community
