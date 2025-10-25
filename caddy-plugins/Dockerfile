ARG CADDY_VERSION=2.10.2
ARG PLUGINS_VERSION=2

FROM caddy:builder AS builder

ARG CADDY_VERSION

# Fetch default configuration and welcome files
RUN git clone --depth 1 --branch v${CADDY_VERSION} https://github.com/caddyserver/dist.git /dist

RUN xcaddy build v${CADDY_VERSION} \
  --with github.com/mholt/caddy-l4@2e3e6cf60b25186d29e3b07e269563f870b36c96 \
  --with github.com/caddy-dns/cloudflare@2fc25ee62f40fe21b240f83ab2fb6e2be6dbb953

FROM alpine:3.22.2

ARG CADDY_VERSION
ARG PLUGINS_VERSION

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
  /usr/share/caddy

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
COPY --from=builder /dist/config/Caddyfile /etc/caddy/Caddyfile
COPY --from=builder /dist/welcome/index.html /usr/share/caddy/index.html

RUN set -eux; \
  setcap cap_net_bind_service=+ep /usr/bin/caddy; \
  chmod +x /usr/bin/caddy; \
  caddy version

ENV XDG_CONFIG_HOME=/config
ENV XDG_DATA_HOME=/data
ENV CADDY_VERSION=v${CADDY_VERSION}

EXPOSE 80
EXPOSE 443
EXPOSE 443/udp
EXPOSE 2019

WORKDIR /srv

CMD ["caddy", "run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
