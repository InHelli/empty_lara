FROM nginx:1.15.12-alpine

ADD nginx.conf /etc/nginx/

ARG PHP_UPSTREAM_CONTAINER

ARG PUID
ENV PUID ${PUID}
ARG PGID
ENV PGID ${PGID}

RUN apk update \
    && apk upgrade \
    && apk add --no-cache bash \
    && adduser -D -H -u ${PUID} -s /bin/bash www-data \
    && rm /etc/nginx/conf.d/default.conf \
    && echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}:9000; }" > /etc/nginx/conf.d/upstream.conf

CMD ["nginx"]

EXPOSE 80 443
