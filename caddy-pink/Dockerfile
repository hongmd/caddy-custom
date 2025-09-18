# syntax=docker/dockerfile:1
ARG CADDY_VERSION=2.10.0
FROM caddy:${CADDY_VERSION}-builder-alpine AS builder

RUN xcaddy build \
    --with github.com/caddy-dns/cloudflare \
    --with github.com/WeidiDeng/caddy-cloudflare-ip \
    --with github.com/mholt/caddy-dynamicdns \
    --with github.com/mholt/caddy-ratelimit \
    --with github.com/hslatman/caddy-crowdsec-bouncer \
    --with github.com/porech/caddy-maxmind-geolocation \
    --with github.com/Odyssey346/ListenCaddy \
    --with github.com/ueffel/caddy-tls-format \
FROM caddy:${CADDY_VERSION}-alpine

COPY --from=builder /usr/bin/caddy /usr/bin/caddy