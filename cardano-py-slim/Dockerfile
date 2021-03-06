ARG _CARDANO_NODE_VERSION=1.25.1
FROM floydcraft/cardano-node-iohk-dev:$_CARDANO_NODE_VERSION as base

FROM python:3.9-slim
LABEL maintainer="chbfiv@floydcraft.com"

ARG _VERSION=0.1.7-dev6

ARG DEBIAN_FRONTEND=noninteractive

ENV ENV=/etc/profile \
    NODE_HOME=/opt/cardano-node \
    PATH=/opt/cardano-node/scripts:/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin

USER root
WORKDIR /

COPY --from=base /root/.cabal/bin/* /usr/local/bin/
COPY --from=base /lib/x86_64-linux-gnu/lib* /lib/x86_64-linux-gnu/
COPY --from=base /lib64/ld-linux-x86-64* /lib64/
COPY --from=base /usr/lib/x86_64-linux-gnu/libgmp.* /usr/lib/x86_64-linux-gnu/
COPY --from=base /usr/lib/x86_64-linux-gnu/liblz4.* /usr/lib/x86_64-linux-gnu/
COPY --from=base /usr/lib/x86_64-linux-gnu/libsodium.* /usr/lib/x86_64-linux-gnu/
COPY --from=base /opt/ /opt/

RUN addgroup -gid 1000 ada \
    && mkdir -p /home/ada \
    && adduser --uid 1000 --disabled-password --shell /bin/bash --no-create-home --gecos '' --home /home/ada --ingroup ada ada \
    && apt-get update \
    && apt-get install --no-install-recommends -y ca-certificates acl libcap2-bin ncurses-bin iproute2 curl wget xz-utils netbase sudo coreutils dnsutils net-tools procps tcptraceroute bc \
    && apt-get -y clean \
    && apt-get -y autoremove \
    && rm -rf /var/lib/apt/lists/* \
    && setfacl -Rdm group:ada:rwx  /home/ada \
    && python -m pip install cardanopy==$_VERSION --no-cache-dir

WORKDIR /home/ada

EXPOSE 3001 12798 12788
ENTRYPOINT [ "bin/bash" ]
