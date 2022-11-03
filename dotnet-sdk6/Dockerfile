FROM marcosoliveirasoares94/jenkins-inbound-agent:base-toolkit

LABEL maintainer="Marcos Oliveira Soares <marcosoliveirasoares94@gmail.com>"

USER root

# Install additional packages
RUN apk add --update --no-cache \
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
RUN apk add --update libgdiplus --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing --allow-untrusted
RUN apk add --update lttng-ust  --no-cache --repository https://dl-cdn.alpinelinux.org/alpine/edge/main --allow-untrusted

# Install additional package dotnet
RUN mkdir -p /usr/share/dotnet \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet
RUN wget https://dot.net/v1/dotnet-install.sh
RUN chmod +x dotnet-install.sh
RUN ./dotnet-install.sh -c 6.0 --install-dir /usr/share/dotnet

RUN dotnet tool install --tool-path /bin dotnet-sonarscanner

LABEL maintainer="Marcos Oliveira Soares <marcosoliveirasoares94@gmail.com>" \
  org.label-schema.build-date=$BUILD_DATE \
  org.label-schema.vcs-ref=$VCS_REF \
  org.label-schema.vcs-url=$VCS_URL \
  org.label-schema.version=$VERSION