FROM caddy:2.10.2-builder AS builder

RUN xcaddy build \
    --with github.com/mholt/caddy-l4@2e3e6cf60b25186d29e3b07e269563f870b36c96 \
    --with github.com/caddy-dns/cloudflare@v0.2.1

FROM caddy:2.10.2

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
