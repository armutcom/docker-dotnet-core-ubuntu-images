FROM armutcom/dotnet-core-runtime:2.1.2-alpine

ENV ASPNETCORE_URLS http://+:80
ENV ASPNETCORE_VERSION 2.1.2
ENV ASPNETCORE_DOWNLOAD_URL https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$ASPNETCORE_VERSION/aspnetcore-runtime-$ASPNETCORE_VERSION-linux-musl-x64.tar.gz

RUN apk add --no-cache --virtual .build-deps \
    openssl \
    && wget -O aspnetcore.tar.gz $ASPNETCORE_DOWNLOAD_URL \
    && aspnetcore_sha512='7e4ccfd0610949c4e228da38d40223ea8086bc98ec92d83fb0823909f40e897c3ad59706ff84e4dfec7873a587e83d9d98826dd13dc2bbea239a927f6906a9cb' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && export DOTNET_HOME=$(dirname $(readlink $(which dotnet))) \
    && tar -x -C $DOTNET_HOME -f aspnetcore.tar.gz \
    && rm aspnetcore.tar.gz \
    && apk del .build-deps

LABEL authors="Armut.com <dev@armut.com>"