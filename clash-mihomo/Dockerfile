FROM alpine:latest AS downloader

WORKDIR /app

ARG VERSION

ARG TARGETOS=linux
ARG TARGETARCH=amd64

RUN : \
    && set -ex \
    && apk add -U --no-cache curl unzip \
    && curl -sSL --retry 10 \
        "https://github.com/MetaCubeX/mihomo/releases/download/v${VERSION}/mihomo-${TARGETOS}-${TARGETARCH}-v${VERSION}.gz" \
        | gzip -d -c > /usr/local/bin/mihomo \
    && chmod +x /usr/local/bin/mihomo \
    && ls -alh /usr/local/bin \
    && :

RUN : \
    && set -ex \
    && mkdir /mihomo-config \
    # && wget -O /mihomo-config/geoip.metadb https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.metadb \
    # && wget -O /mihomo-config/geosite.dat https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat \
    # && wget -O /mihomo-config/geoip.dat https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat \
    && wget -O /mihomo-config/geoip.metadb https://cdn.jsdelivr.net/gh/Loyalsoldier/geoip@release/Country.mmdb \
    && wget -O /mihomo-config/GeoLite2-ASN.mmdb https://cdn.jsdelivr.net/gh/Loyalsoldier/geoip@release/GeoLite2-ASN.mmdb \
    && wget -O /mihomo-config/geoip.dat https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat \
    && wget -O /mihomo-config/geosite.dat https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat \
    && :

FROM gcr.io/distroless/static-debian12:latest

LABEL maintainer="Sunlei <guizaicn@gmail.com>"
LABEL repository="https://github.com/sunlei/dockerfiles"
LABEL description="Dockerfiles I am using"

LABEL org.opencontainers.image.authors="Sunlei <guizaicn@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/sunlei/dockerfiles"
LABEL org.opencontainers.image.description="Dockerfiles I am using"

COPY --from=downloader /mihomo-config/ /etc/mihomo/
COPY --from=downloader /usr/local/bin/mihomo /mihomo

ENTRYPOINT [ "/mihomo", "-d", "/etc/mihomo" ]
