FROM fluent/fluentd:v1.7.1-1.0

LABEL maintainer="nakatest1234, twitter:@ambient2step" \
      Description="Fluentd docker custom image"

ENV FLUENT_TZ      Asia/Tokyo
ENV FLUENT_PLUGINS fluent-plugin-s3:1.1.11 fluent-plugin-slack:0.6.7 fluent-plugin-record-reformer:0.9.1

USER root

RUN apk update -q --no-cache ca-certificates \
    && apk add -q --no-cache tzdata \
    && cp -f /usr/share/zoneinfo/${FLUENT_TZ} /etc/localtime \
    && gem install -N --quiet --silent ${FLUENT_PLUGINS} \
    && apk del -q tzdata \
    && rm -rf /usr/local/bundle/cache/*.gem \
    && mkdir -p /var/log/fluent && chown fluent:fluent /var/log/fluent \
    && :

COPY fluent.conf /fluentd/etc/fluent.conf

USER fluent
