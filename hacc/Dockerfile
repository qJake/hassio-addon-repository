ARG BUILD_FROM=hassioaddons/base:4.0.2
ARG HACC_VERSION=0.4.3

FROM ${BUILD_FROM} AS base

# ASP.NET Core 3.x Runtime
FROM mcr.microsoft.com/dotnet/core/aspnet:3.0-alpine3.9-arm64v8 AS dotnet

WORKDIR /

RUN apk update && apk add wget tar

RUN mkdir /app \
    && cd /app \
    && wget -qO app.tar.gz https://github.com/qJake/HADotNet.CommandCenter/releases/download/v$HACC_VERSION/HADotNet.CommandCenter-armhf.tar.gz \
    && tar xzf app.tar.gz \
    && rm -rf app.tar.gz
	
# Pull publish into runtime
FROM base AS final

## Copy dotnet runtime over
WORKDIR /

COPY --from=dotnet /usr/share/dotnet /usr/share/dotnet

RUN ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

COPY --from=dotnet /app .

ENV ASPNETCORE_URLS http://*:8095
ENV ASPNETCORE_ENVIRONMENT Production
ENV HACC_VERSION $HACC_VERSION

# Pull .NET stuff into Hassio Base

EXPOSE 8095

ENV LANG C.UTF-8

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
    org.label-schema.name="HACC" \
    org.label-schema.schema-version="$HACC_VERSION" \
    org.label-schema.url="https://github.com/qJake/HADotNet.Panel/" \
    org.label-schema.usage="https://github.com/qJake/HADotNet.Panel/README.md" \
    org.label-schema.vcs-ref=${BUILD_REF} \
    org.label-schema.vcs-url="https://github.com/qJake/HADotNet.Panel/" \
    org.label-schema.vendor="qJake"
