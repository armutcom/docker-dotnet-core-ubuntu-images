FROM alpine:3.8

# procps is very common in build systems, and is a reasonably small package
RUN apk add --no-cache \
    bzr \
    git \
    mercurial \
    openssh-client \
    subversion \
    \
    procps \
    && rm -rf /var/lib/apt/lists/*

RUN apk add --no-cache icu-libs \
    ca-certificates \
    \
    # .NET Core dependencies
    krb5-libs \
    libgcc \
    libintl \
    libssl1.0 \
    libstdc++ \
    tzdata \
    userspace-rcu \
    zlib \
    && apk -X https://dl-cdn.alpinelinux.org/alpine/edge/main add --no-cache \
    lttng-ust \
    icu-dev \
    && rm -rf /var/lib/apt/lists/*


ENV DOTNET_SDK_VERSION 2.1.302
ENV DOTNET_SDK_DOWNLOAD_URL https://dotnetcli.blob.core.windows.net/dotnet/Sdk/$DOTNET_SDK_VERSION/dotnet-sdk-$DOTNET_SDK_VERSION-linux-musl-x64.tar.gz
ENV DOTNET_SDK_DOWNLOAD_SHA 0f9a6fcbad609ef1ff5b398de9a1f1bf59eebc59b28a4c8cfead28f0209bf77601d05d49f5ea1223c860a803fb82cd7e2401b6df290da34e54b36bdd8788ed48

RUN apk add --no-cache --virtual .build-deps \
    openssl \
    && wget -O dotnet.tar.gz $DOTNET_SDK_DOWNLOAD_URL \
    && echo "$DOTNET_SDK_DOWNLOAD_SHA  dotnet.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -C /usr/share/dotnet -xzf dotnet.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
    && rm dotnet.tar.gz \
    && apk del .build-deps

# Enable detection of running in a container
ENV DOTNET_RUNNING_IN_CONTAINER=true \
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true \
    # Skip extraction of XML docs - generally not useful within an image/container - helps perfomance
    NUGET_XMLDOC_MODE=skip

# Trigger the population of the local package cache
RUN mkdir warmup \
    && cd warmup \
    && dotnet new \
    && cd .. \
    && rm -rf warmup \
    && rm -rf /tmp/NuGetScratch

# Workaround for https://github.com/Microsoft/DockerTools/issues/87. This instructs NuGet to use 4.5 behavior in which
# all errors when attempting to restore a project are ignored and treated as warnings instead. This allows the VS
# tooling to use -nowarn:MSB3202 to ignore issues with the .dcproj project
ENV RestoreUseSkipNonexistentTargets false

LABEL authors="Armut.com <dev@armut.com>"