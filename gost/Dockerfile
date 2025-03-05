FROM alpine:latest AS downloader

WORKDIR /app

ARG VERSION

ARG TARGETOS=linux
ARG TARGETARCH=amd64

RUN : \
    && set -ex \
    && apk add -U --no-cache curl unzip \
    && curl -sSL --retry 10 \
        "https://github.com/go-gost/gost/releases/download/v${VERSION}/gost_${VERSION}_${TARGETOS}_${TARGETARCH}.tar.gz" \
        | tar -xz --directory /usr/local/bin --no-same-owner gost \
    && ls -alh /usr/local/bin \
    && :


FROM gcr.io/distroless/static-debian12:latest

LABEL maintainer="Sunlei <guizaicn@gmail.com>"
LABEL repository="https://github.com/sunlei/dockerfiles"
LABEL description="Dockerfiles I am using"

LABEL org.opencontainers.image.authors="Sunlei <guizaicn@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/sunlei/dockerfiles"
LABEL org.opencontainers.image.description="Dockerfiles I am using"

COPY --from=downloader /usr/local/bin/gost /usr/local/bin/gost

ENTRYPOINT ["/usr/local/bin/gost"]
