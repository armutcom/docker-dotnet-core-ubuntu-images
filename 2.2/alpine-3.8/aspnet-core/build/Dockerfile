FROM armutcom/dotnet-core-sdk:2.1.302-runtime-2.1.2-alpine

ENV ASPNETCORE_URLS http://+:80
ENV ASPNETCORE_PKG_VERSION 2.1.2

ENV NODE_VERSION 8.11.2
ENV NODE_DOWNLOAD_SHA 539946c0381809576bed07424a35fc1740d52f4bd56305d6278d9e76c88f4979
ENV NODE_DOWNLOAD_URL https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION.tar.xz

RUN apk add --no-cache --virtual .build-deps \
    binutils-gold \
    g++ \
    gcc \
    gnupg \
    libgcc \
    linux-headers \
    make \
    python \
    && wget -O nodejs.tar.xz $NODE_DOWNLOAD_URL \
    && echo "$NODE_DOWNLOAD_SHA  nodejs.tar.xz" | sha256sum -c - \
    && tar -C /usr/local -xf nodejs.tar.xz \
    && cd /usr/local/node-v$NODE_VERSION \
    && ./configure \
    && make -j$(getconf _NPROCESSORS_ONLN) \
    && make install \
    && apk del .build-deps \
    && cd .. \
    && ls -al \
    && rm -Rf "node-v$NODE_VERSION" \
    # set up bower and gulp
    && npm install -g bower gulp \
    && echo '{ "allow_root": true }' > /root/.bowerrc \
    && rm -Rf nodejs.tar.xz 

# warmup NuGet package cache
COPY packagescache.csproj /tmp/warmup/
RUN dotnet restore /tmp/warmup/packagescache.csproj \
    --source https://api.nuget.org/v3/index.json \
    --verbosity quiet \
    && rm -rf /tmp/warmup/

WORKDIR /

LABEL authors="Armut.com <dev@armut.com>"