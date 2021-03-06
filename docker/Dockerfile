# -- build-environment --
# see https://docs.docker.com/engine/userguide/eng-image/multistage-build/

FROM erlang:22.2.7-alpine AS build-env

ARG     BUILD_PROFILE=prod
WORKDIR /build
RUN     apk update && apk --no-cache upgrade && \
		apk --no-cache add \
		gcc \
		git \
		libc-dev libc-utils \
		libgcc \
		linux-headers \
		make bash \
		musl-dev musl-utils \
		ncurses-dev \
		pcre2 \
		pkgconf \
		scanelf \
		wget \
		zlib

RUN     wget -O /usr/local/bin/rebar3 https://s3.amazonaws.com/rebar3/rebar3 && \
		chmod a+x /usr/local/bin/rebar3

ADD     . /build
RUN     export BUILD_PROFILE=$(echo $BUILD_PROFILE | sed 's/+/,/g') && \
	echo "build as $BUILD_PROFILE" && \
	rebar3 as $BUILD_PROFILE release

# -- runtime image --

FROM    alpine:3.11

ARG     BUILD_PROFILE=prod
WORKDIR /
RUN     apk add --no-cache -U zlib ncurses-libs libcrypto1.1 lksctp-tools tini
COPY    docker/docker-entrypoint.sh /
COPY    config/ergw-capwap-node.config /etc/ergw-capwap-node/

RUN     mkdir -p /var/lib/ergw/ && \
		touch /var/lib/ergw/ergw.state

COPY    --from=build-env /build/_build/${BUILD_PROFILE}/rel/ /opt/

ENTRYPOINT ["tini", "--", "/docker-entrypoint.sh"]
CMD     ["/opt/ergw-capwap-node/bin/ergw-capwap-node", "foreground"]
