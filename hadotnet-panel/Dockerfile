ARG BUILD_FROM=hassioaddons/base:4.0.2
ARG REPO=mcr.microsoft.com/dotnet/core/runtime-deps

# .NET Core 2.2.6 for Alpine - Download and Install

FROM $REPO:2.2-alpine3.9 AS bld

ENV ASPNETCORE_VERSION 2.2.6
ENV ASPNETCORE_URLS http://+:9095

RUN wget -O aspnetcore.tar.gz https://dotnetcli.blob.core.windows.net/dotnet/aspnetcore/Runtime/$ASPNETCORE_VERSION/aspnetcore-runtime-$ASPNETCORE_VERSION-linux-musl-x64.tar.gz \
    && aspnetcore_sha512='590e73898a6f1b3ef7b86e019b681a3099b7046b02cdb7f99dea61460aa6079211a42af3587bf71c03c19655dc24455098b14b3e9a9a12aeb2e64b7ecc207f84' \
    && echo "$aspnetcore_sha512  aspnetcore.tar.gz" | sha512sum -c - \
    && mkdir -p /usr/share/dotnet \
    && tar -zxf aspnetcore.tar.gz -C /usr/share/dotnet \
    && rm aspnetcore.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet \
    && apk add --no-cache git \
    && mkdir /src \
    && cd /src \
    && wget -O release.tar.gz https://github.com/qJake/HADotNet.Panel/releases/download/v0.0.3-alpha/HADotNet.Panel.tar.gz \
    && tar xzf release.tar.gz \
    && rm -rf release.tar.gz

# Hass.io Add-on Container

FROM ${BUILD_FROM}

ENV LANG C.UTF-8

# Copy .NET Runtime over

WORKDIR /

COPY --from=bld /usr/share/dotnet /usr/share/dotnet

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

WORKDIR /src

CMD ["dotnet", "HADotNet.Panel.dll"]

# Build arguments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="HAPanel" \
    io.hass.description="Wall-mountable panels for Home Assistant." \
    io.hass.arch="armv7|i386|amd64" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="qJake <https://github.com/qJake/>" \
    org.label-schema.description="Wall-mountable panels for Home Assistant." \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="HAPanel" \
    org.label-schema.schema-version="0.2" \
    org.label-schema.url="https://github.com/qJake/HADotNet.Panel/" \
    org.label-schema.usage="https://github.com/qJake/HADotNet.Panel/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/qJake/HADotNet.Panel/" \
    org.label-schema.vendor="qJake"
