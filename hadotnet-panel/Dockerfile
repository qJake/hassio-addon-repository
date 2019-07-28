ARG BUILD_FROM=hassioaddons/base:4.0.2
FROM ${BUILD_FROM}

ENV LANG C.UTF-8

ENV ASPNETCORE_VERSION 2.2.6
ENV ASPNETCORE_URLS http://+:9095

RUN wget -O aspnetcore.tar.gz https://download.visualstudio.microsoft.com/download/pr/c3257647-9887-4822-8a18-0debbeabd3b2/5cd0346e78464231883604e544cee8dd/aspnetcore-runtime-2.2.6-linux-musl-x64.tar.gz \
    && mkdir -p /usr/share/dotnet \
    && tar -zxf aspnetcore.tar.gz -C /usr/share/dotnet \
    && rm aspnetcore.tar.gz \
    && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet

RUN mkdir /src \
    && cd /src \
    && wget -O release.tar.gz https://github.com/qJake/HADotNet.Panel/releases/download/v0.0.3-alpha/HADotNet.Panel.tar.gz \
    && tar xzf release.tar.gz \
    && rm -rf release.tar.gz

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
