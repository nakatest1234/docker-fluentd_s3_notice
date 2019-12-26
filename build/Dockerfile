FROM fluent/fluentd:v1.7.1-1.0

MAINTAINER nakatest1234 <ambient2step@gmail.com>

USER root

RUN apk update -q ca-certificates \
    && apk add -q --no-cache tzdata \
    && cp -f /usr/share/zoneinfo/Asia/Tokyo /etc/localtime \
    && gem install -N --quiet --silent \
        fluent-plugin-s3:1.1.11 \
        fluent-plugin-slack:0.6.7 \
        fluent-plugin-record-reformer:0.9.1 \
    && apk del -q tzdata \
    && rm -rf /usr/local/bundle/cache/*.gem \
    && mkdir -p /var/log/fluent && chown fluent:fluent /var/log/fluent \
    && :

COPY fluent.conf /fluentd/etc/fluent.conf

USER fluent
