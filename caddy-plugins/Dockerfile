ARG CADDY_VERSION=2.11.0-beta.2
ARG PLUGINS_VERSION=2.2
ARG L4_PLUGIN_COMMIT=5b50eb6ed84c74cd3d4b15f854b93b8b6d71424c
ARG CLOUDFLARE_PLUGIN_COMMIT=6dc1fbb7e925b0da6736780db2e90f4816b79ab7
ARG CADDY_DIST_BASE="https://github.com/caddyserver/dist/raw/refs/tags/v${CADDY_VERSION}"

FROM caddy:builder AS builder

ARG CADDY_VERSION
ARG L4_PLUGIN_COMMIT
ARG CLOUDFLARE_PLUGIN_COMMIT

# Fetch default configuration and welcome files
# RUN git clone --depth 1 --branch v${CADDY_VERSION} https://github.com/caddyserver/dist.git /dist

RUN xcaddy build v${CADDY_VERSION} \
  --with github.com/mholt/caddy-l4@${L4_PLUGIN_COMMIT} \
  --with github.com/caddy-dns/cloudflare@${CLOUDFLARE_PLUGIN_COMMIT}

FROM alpine:3.23

ARG CADDY_VERSION
ARG PLUGINS_VERSION
ARG CADDY_DIST_BASE

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
  /usr/share/caddy; \
  wget -q -O /etc/caddy/Caddyfile "${CADDY_DIST_BASE}/config/Caddyfile"; \
  wget -q -O /usr/share/caddy/index.html "${CADDY_DIST_BASE}/welcome/index.html"

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
# COPY --from=builder /dist/config/Caddyfile /etc/caddy/Caddyfile
# COPY --from=builder /dist/welcome/index.html /usr/share/caddy/index.html

RUN set -eux; \
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
