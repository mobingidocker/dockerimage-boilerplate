# this file is created by rake exprt task.
FROM alpine:3.5
LABEL com.mobingi.code_dir="/var/www/html"

RUN apk add tar bash curl wget git --no-cache

RUN curl -sfL \
  https://github.com/jwilder/dockerize/releases/download/v0.6.1/dockerize-alpine-linux-amd64-v0.6.1.tar.gz \
  | tar xvzf - -C /


ADD https://github.com/just-containers/s6-overlay/releases/download/v1.21.2.1/s6-overlay-amd64.tar.gz /tmp/
RUN tar xzf /tmp/s6-overlay-amd64.tar.gz -C / && rm -f /tmp/s6-overlay-amd64.tar.gz

ADD ./s6-defaults/fix-attrs.d /etc/fix-attrs.d
ADD ./s6-defaults/cont-init.d /etc/cont-init.d
ADD ./s6-defaults/services.d /etc/services.d
ADD ./s6-defaults/cont-finish.d /etc/cont-finish.d

## merge s6-dirs
ONBUILD ADD ./fix-attrs.d /etc/fix-attrs.d
ONBUILD ADD ./cont-init.d /etc/cont-init.d
ONBUILD ADD ./services.d /etc/services.d
ONBUILD ADD ./cont-finish.d /etc/cont-finish.d

HEALTHCHECK --start-period=3m --interval=1m --timeout=5s \
  CMD curl -f http://127.0.0.1/ || exit 1

ENTRYPOINT ["/init"]
