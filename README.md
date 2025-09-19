# caddy-custom

[![Build caddy-pink](https://github.com/hongmd/caddy-custom/actions/workflows/build.yml/badge.svg)](https://github.com/hongmd/caddy-custom/actions/workflows/build.yml)

Custom Docker build that extends the official Caddy image with a set of opinionated modules for Cloudflare-friendly deployments.

## Included modules
- `github.com/caddy-dns/cloudflare` for Cloudflare-managed DNS challenges
- `github.com/WeidiDeng/caddy-cloudflare-ip` to keep up-to-date Cloudflare IP ranges
- `github.com/mholt/caddy-dynamicdns` for pushing records to supported dynamic DNS providers
- `github.com/mholt/caddy-ratelimit` to throttle traffic per route
- `github.com/hslatman/caddy-crowdsec-bouncer` to integrate with CrowdSec decisions
- `github.com/porech/caddy-maxmind-geolocation` to expose MaxMind GeoIP metadata
- `github.com/Odyssey346/ListenCaddy` to monitor active listeners via HTTP
- `github.com/ueffel/caddy-tls-format` to export TLS certificates and keys in PEM/DER formats

## Building the image
```bash
# from the repository root
cd caddy-pink
docker build -t my-caddy .
```

Use the `CADDY_VERSION` build argument in the `Dockerfile` to pin the upstream Caddy release:
```bash
docker build --build-arg CADDY_VERSION=2.10.2 -t my-caddy .
```

## Running the container
Mount your `Caddyfile` (and optional data/config directories) the same way you would with the upstream image:
```bash
docker run --rm \
  -p 80:80 -p 443:443 \
  -v $(pwd)/Caddyfile:/etc/caddy/Caddyfile \
  -v caddy_data:/data \
  -v caddy_config:/config \
  my-caddy
```

All added modules are available immediately—configure them in your `Caddyfile` according to their documentation.

## Modifying the build
To add or remove modules, edit the `xcaddy build` statement in `caddy-pink/Dockerfile` and rebuild the image. Refer to the [xcaddy documentation](https://caddyserver.com/docs/build) for syntax details.

## License
Distributed under the terms of the GNU General Public License v3.0. See `LICENSE` for details.
