FROM alpine:latest AS downloader

WORKDIR /app

ARG TARGETOS=linux
ARG TARGETARCH=amd64

ENV SNELL_SERVER_VERSION=v4.1.1
ENV OUTPUT_FILE=snell-server.zip

RUN : \
    && set -ex \
    && apk add -U --no-cache curl unzip \
    && wget -q "https://dl.nssurge.com/snell/snell-server-${SNELL_SERVER_VERSION}-${TARGETOS}-${TARGETARCH}.zip" --output-document="${OUTPUT_FILE}" \
    && unzip "${OUTPUT_FILE}" \
    && :

FROM gcr.io/distroless/static-debian12:latest

LABEL maintainer="Sunlei <guizaicn@gmail.com>"
LABEL repository="https://github.com/sunlei/dockerfiles"
LABEL description="Dockerfiles I am using"

LABEL org.opencontainers.image.authors="Sunlei <guizaicn@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/sunlei/dockerfiles"
LABEL org.opencontainers.image.description="Dockerfiles I am using"

COPY --from=downloader /app/snell-server /usr/local/bin/

ENTRYPOINT ["snell-server"]
