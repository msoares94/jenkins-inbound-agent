FROM marcosoliveirasoares94/jenkins-inbound-agent:base-toolkit

LABEL maintainer="Marcos Oliveira Soares <marcosoliveirasoares94@gmail.com>"

USER root

# Install additional packages
RUN apk add --update \
    ca-certificates \
    less \
    ncurses-terminfo-base \
    bash \
    icu-libs \
    krb5-libs \
    libgcc \
    libintl \
    libssl1.1 \
    libstdc++ \
    zlib \
    tzdata \
    userspace-rcu

# Install additional package libgdiplus
RUN apk add --update libgdiplus --repository https://dl-3.alpinelinux.org/alpine/edge/testing/
RUN apk add --update lttng-ust --repository https://dl-cdn.alpinelinux.org/alpine/edge/main

# Install additional package dotnet
RUN mkdir -p /usr/share/dotnet \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
RUN wget https://dot.net/v1/dotnet-install.sh
RUN chmod +x dotnet-install.sh
RUN ./dotnet-install.sh -c 5.0 --install-dir /usr/share/dotnet