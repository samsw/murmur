FROM alpine

ARG BUILD_DATE
ARG VCS_REF
ARG VERSION

ENV \
    DEBUG=false \
    MURMUR_OPTS= \
    PGID=999 \
    PUID=999 \
    PORT=64738 \
    RUN_CHOWN=true

WORKDIR /opt/murmur

COPY root /

RUN set -x \
    && delgroup ping \
    && addgroup -g $PGID murmur \
    && adduser -D -G murmur -u $PUID murmur \
    && apk add -q --no-cache --virtual .build-deps \
        curl \
    && apk add -q --no-cache \
        bash shadow tzdata \
    && curl -sSL https://mumble.info/snapshot/murmur-static_x86-${VERSION}.tar.bz2 -o /tmp/murmur.tar.bz2 \
    && tar -jx -f /tmp/murmur.tar.bz2 --strip-components=1 -C /opt/murmur \
    && bash -c 'mkdir -p {cert,config,data,log}' \
    && mv /opt/murmur/murmur.ini /opt/murmur/murmur.ini-default \
    && chown -R murmur:murmur /opt/murmur \
    && apk del -q --purge .build-deps \
    && rm -rf /tmp/* /var/tmp/* /var/cache/apk/*

EXPOSE $PORT/tcp $PORT/udp

VOLUME ["/opt/murmur/cert","/opt/murmur/config","/opt/murmur/data","/opt/murmur/log"]

ENTRYPOINT ["docker-entrypoint.sh"]

CMD ["murmur"]
