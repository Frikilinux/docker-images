FROM caddy:builder AS builder

ENV CADDY_VERSION=2.10.2

RUN xcaddy build v${CADDY_VERSION} \
    --with github.com/mholt/caddy-l4@2e3e6cf \
    --with github.com/caddy-dns/cloudflare@2fc25ee

FROM alpine:3.22.2

ENV CADDY_VERSION=2.10.2
ENV PLUGINS_VERSION=2

LABEL org.opencontainers.image.version=${CADDY_VERSION}-plugins.${PLUGINS_VERSION}
LABEL org.opencontainers.image.title=Caddy-plugins
LABEL org.opencontainers.image.description="Custom Caddy server with plugins"
LABEL org.opencontainers.image.url="https://github.com/Frikilinux/docker-images/tree/main/caddy-plugins"
LABEL org.opencontainers.image.documentation="https://github.com/Frikilinux/docker-images/tree/main/caddy-plugins"
LABEL org.opencontainers.image.vendor="Zotta DEV"
LABEL org.opencontainers.image.licenses=Apache-2.0
LABEL org.opencontainers.image.source="https://github.com/Frikilinux/docker-images/tree/main/caddy-plugins"

RUN apk add --no-cache ca-certificates libcap mailcap

RUN set -eux; \
    mkdir -p \
		/config/caddy \
		/data/caddy \
		/etc/caddy \
		/usr/share/caddy \
		; \
	wget -O /etc/caddy/Caddyfile "https://github.com/caddyserver/dist/raw/v${CADDY_VERSION}/config/Caddyfile"; \
	wget -O /usr/share/caddy/index.html "https://github.com/caddyserver/dist/raw/v${CADDY_VERSION}/welcome/index.html"

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
RUN set -eux; \
    setcap cap_net_bind_service=+ep /usr/bin/caddy; \
	chmod +x /usr/bin/caddy; \
	caddy version; \
    unset CADDY_VERSION \
        PLUGINS_VERSION

ENV XDG_CONFIG_HOME=/config
ENV XDG_DATA_HOME=/data

EXPOSE 80
EXPOSE 443
EXPOSE 443/udp
EXPOSE 2019

WORKDIR /srv

CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
