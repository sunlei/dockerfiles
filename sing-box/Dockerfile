# syntax=docker/dockerfile:1
FROM golang:1 as builder

ENV CGO_ENABLED=0

RUN <<EOF
    go install -v -tags \
        with_v2ray_api \
    github.com/sagernet/sing-box/cmd/sing-box@latest
EOF


FROM gcr.io/distroless/static-debian11:latest

LABEL maintainer="Sunlei <guizaicn@gmail.com>"
LABEL repository="https://github.com/sunlei/dockerfiles"
LABEL description="Dockerfiles I am using"

LABEL org.opencontainers.image.authors="Sunlei <guizaicn@gmail.com>"
LABEL org.opencontainers.image.source="https://github.com/sunlei/dockerfiles"
LABEL org.opencontainers.image.description="Dockerfiles I am using"

COPY --from=builder /go/bin/sing-box /usr/local/bin/sing-box
ENTRYPOINT ["sing-box"]
