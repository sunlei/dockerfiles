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
    && mkdir /etc/mihomo \
    # && wget -O /etc/mihomo/geoip.metadb https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.metadb \
    # && wget -O /etc/mihomo/geosite.dat https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geosite.dat \
    # && wget -O /etc/mihomo/geoip.dat https://fastly.jsdelivr.net/gh/MetaCubeX/meta-rules-dat@release/geoip.dat \
    && wget --retry-connrefused --waitretry=1 --tries=5 -O /etc/mihomo/geoip.metadb https://raw.githubusercontent.com/Loyalsoldier/geoip/release/Country.mmdb \
    && wget --retry-connrefused --waitretry=1 --tries=5 -O /etc/mihomo/GeoLite2-ASN.mmdb https://raw.githubusercontent.com/Loyalsoldier/geoip/release/GeoLite2-ASN.mmdb \
    && wget --retry-connrefused --waitretry=1 --tries=5 -O /etc/mihomo/geoip.dat https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geoip.dat \
    && wget --retry-connrefused --waitretry=1 --tries=5 -O /etc/mihomo/geosite.dat https://github.com/Loyalsoldier/v2ray-rules-dat/releases/latest/download/geosite.dat \
    && :

FROM gcr.io/distroless/static-debian12:latest

LABEL maintainer="Sunlei <guizaicn@gmail.com>"
LABEL repository="https://github.com/sunlei/dockerfiles"
LABEL description="Dockerfiles I am using"

LABEL org.opencontainers.image.authors="Sunlei <guizaicn@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/sunlei/dockerfiles"
LABEL org.opencontainers.image.description="Dockerfiles I am using"

COPY --from=downloader /etc/mihomo/ /etc/mihomo/
COPY --from=downloader /usr/local/bin/mihomo /usr/local/bin/mihomo

ENTRYPOINT [ "/usr/local/bin/mihomo", "-d", "/etc/mihomo" ]
