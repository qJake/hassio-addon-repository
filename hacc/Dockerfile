ARG BUILD_FROM=hassioaddons/base:4.0.2

# ASP.NET Core 2.2.x Runtime
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-alpine3.9-arm64v8 AS base
WORKDIR /app

# ASP.NET Core 2.2.x SDK and Build
FROM mcr.microsoft.com/dotnet/core/sdk:3.0-alpine3.9 AS build
ARG TAG=master

ENV HACC_VERSION $TAG

WORKDIR /

RUN apk update && apk add wget nodejs npm tar

RUN npm install -g npm

RUN mkdir /src \
    && cd /src \
    && wget -qO src.tar.gz https://github.com/qJake/HADotNet.CommandCenter/archive/$TAG.tar.gz \
    && tar xzf src.tar.gz --strip=1 --exclude pax_global_header \
    && rm -rf src.tar.gz

WORKDIR /src/HADotNet.CommandCenter

RUN dotnet restore HADotNet.CommandCenter.csproj

RUN npm install

RUN dotnet build HADotNet.CommandCenter.csproj -c Release -o /app

# Publish Command
FROM build AS publish
RUN dotnet publish HADotNet.CommandCenter.csproj -c Release -o /app

# Pull publish into runtime
FROM base AS final
ARG TAG=master

WORKDIR /app
COPY --from=publish /app .

ENV ASPNETCORE_URLS http://*:8095
ENV ASPNETCORE_ENVIRONMENT Production
ENV HACC_VERSION $TAG

# Pull .NET stuff into Hassio Base
FROM ${BUILD_FROM}

EXPOSE 8095

ENV LANG C.UTF-8

ENV ASPNETCORE_VERSION 2.2.6
ENV ASPNETCORE_URLS http://+:9095

WORKDIR /

COPY --from=final /usr/share/dotnet /usr/share/dotnet
COPY --from=final /app /app

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

WORKDIR /app

CMD ["dotnet", "HADotNet.CommandCenter.dll"]

# Build arguments
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="HACC" \
    io.hass.description="Wall-mountable tablet interface for Home Assistant." \
    io.hass.arch="armv7|i386|amd64" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="qJake <https://github.com/qJake/>" \
    org.label-schema.description="Wall-mountable tablet interface for Home Assistant." \
    org.label-schema.build-date=${BUILD_DATE} \
    org.label-schema.name="HAPanel" \
    org.label-schema.schema-version="0.4.1" \
    org.label-schema.url="https://github.com/qJake/HADotNet.Panel/" \
    org.label-schema.usage="https://github.com/qJake/HADotNet.Panel/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/qJake/HADotNet.Panel/" \
    org.label-schema.vendor="qJake"
