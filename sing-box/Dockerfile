FROM golang:1 as builder

ENV CGO_ENABLED=0
ENV BUILD_TAGS=with_v2ray_api,with_gvisor,with_quic,with_dhcp,with_wireguard,with_utls,with_reality_server,with_clash_api,with_acme

ARG LATEST_VERSION

# sing-box@latest / sing-box@dev-next / sing-box@${LATEST_VERSION}
RUN : \
    && set -ex \
    && go install -v \
        -trimpath \
        -tags ${BUILD_TAGS} \
        -ldflags " -s -w -buildid=" \
        github.com/sagernet/sing-box/cmd/sing-box@${LATEST_VERSION} \
    && :

# ARG LATEST_VERSION
# WORKDIR /go/src/github.com/sagernet/sing-box
# RUN <<EOF
#     set -ex
#     apt-get update -qq
#     apt-get install -y --no-install-recommends jq git

#     # LATEST_VERSION=$(curl -sfL "https://api.github.com/repos/SagerNet/sing-box/releases" | jq -r 'first | .tag_name')
#     git clone --depth=1 -b "${LATEST_VERSION}" https://github.com/SagerNet/sing-box.git /go/src/github.com/sagernet/sing-box

#     export COMMIT=$(git rev-parse --short HEAD)

#     go build -v -trimpath \
#         -tags ${BUILD_TAGS} \
#         -o /go/bin/sing-box \
#         -ldflags "-s -w -buildid=" \
#         ./cmd/sing-box
# EOF

# --------------------------------------------------

FROM busybox:1-musl AS busybox

# --------------------------------------------------

FROM gcr.io/distroless/static-debian11:latest

LABEL maintainer="Sunlei <guizaicn@gmail.com>"
LABEL repository="https://github.com/sunlei/dockerfiles"
LABEL description="Dockerfiles I am using"

LABEL org.opencontainers.image.authors="Sunlei <guizaicn@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/sunlei/dockerfiles"
LABEL org.opencontainers.image.description="Dockerfiles I am using"

COPY --from=busybox /bin/wget /usr/bin/wget
COPY --from=builder /go/bin/sing-box /usr/local/bin/sing-box

ENTRYPOINT ["sing-box"]
