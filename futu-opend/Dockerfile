FROM ubuntu:24.04 AS downloader

# NOTE: OpenD 好像只支持 16.04

ENV OPEND_VERSION=9.4.5418
ENV OPEND_FULL_VERSION=Futu_OpenD_${OPEND_VERSION}_Ubuntu16.04

RUN : \
    && set -ex \
    # 安装相关包
    && apt-get update \
    && apt-get install -y --no-install-recommends \
        wget \
    && :

RUN : \
    && set -ex \
    && mkdir -p /tmp/opend \
    # && wget --no-check-certificate -O /tmp/opend.tar.gz "https://www.futunn.com/download/fetch-lasted-link?name=opend-ubuntu" \
    && wget -q --no-check-certificate -O /tmp/opend.tar.gz "https://softwaredownload.futunn.com/${OPEND_FULL_VERSION}.tar.gz" \
    && tar -zxvf /tmp/opend.tar.gz --directory /tmp/opend --no-same-owner \
    && mv /tmp/opend/${OPEND_FULL_VERSION}/${OPEND_FULL_VERSION}/ /usr/local/bin/opend/ \
    && rm -rf /tmp/opend \
    && rm -rf /tmp/opend.tar.gz \
    && :

RUN : \
    && set -ex \
    && ls -alh /usr/local/bin/opend \
    && :


FROM ubuntu:24.04

COPY --from=downloader /usr/local/bin/opend/ /usr/local/bin/opend/

ENTRYPOINT ["/usr/local/bin/opend/FutuOpenD"]
