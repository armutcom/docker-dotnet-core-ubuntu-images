FROM alpine:3.8

RUN apk add --no-cache \
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
    icu-dev

ENV DOTNET_RUNNING_IN_CONTAINER=true

LABEL authors="Armut.com <dev@armut.com>"