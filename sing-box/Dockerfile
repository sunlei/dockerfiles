# syntax=docker/dockerfile:1
FROM golang:1 as builder

ENV CGO_ENABLED=0

WORKDIR /go/src/github.com/sagernet/sing-box

RUN <<EOF
    apt-get update -qq
    apt-get install -y --no-install-recommends jq git

    LATEST_VERSION=$(curl -sfL "https://api.github.com/repos/SagerNet/sing-box/releases" | jq -r 'first | .tag_name')
    git clone --depth=1 -b "${LATEST_VERSION}" https://github.com/SagerNet/sing-box.git /go/src/github.com/sagernet/sing-box

    export COMMIT=$(git rev-parse --short HEAD)

    go build -v -trimpath \
        -tags with_acme,with_v2ray_api \
        -o /go/bin/sing-box \
        -ldflags "-s -w -buildid=" \
        ./cmd/sing-box
EOF

# --------------------------------------------------

FROM gcr.io/distroless/static-debian11:latest

LABEL maintainer="Sunlei <guizaicn@gmail.com>"
LABEL repository="https://github.com/sunlei/dockerfiles"
LABEL description="Dockerfiles I am using"

LABEL org.opencontainers.image.authors="Sunlei <guizaicn@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/sunlei/dockerfiles"
LABEL org.opencontainers.image.description="Dockerfiles I am using"

COPY --from=builder /go/bin/sing-box /usr/local/bin/sing-box

ENTRYPOINT ["sing-box"]
