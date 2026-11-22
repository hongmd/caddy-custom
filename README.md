# caddy-custom

[![Build](https://img.shields.io/github/actions/workflow/status/hongmd/caddy-custom/build.yml?branch=main&label=build)](https://github.com/hongmd/caddy-custom/actions/workflows/build.yml)
[![Examples](https://img.shields.io/github/actions/workflow/status/hongmd/caddy-custom/validate-examples.yml?branch=main&label=examples)](https://github.com/hongmd/caddy-custom/actions/workflows/validate-examples.yml)
[![Release](https://img.shields.io/github/v/release/hongmd/caddy-custom?display_name=tag)](https://github.com/hongmd/caddy-custom/releases)
[![Docker](https://img.shields.io/docker/v/hongmd/caddy-pink?sort=semver&label=docker)](https://hub.docker.com/r/hongmd/caddy-pink)
[![Image size](https://img.shields.io/docker/image-size/hongmd/caddy-pink?label=size)](https://hub.docker.com/r/hongmd/caddy-pink)
[![Pulls](https://img.shields.io/docker/pulls/hongmd/caddy-pink)](https://hub.docker.com/r/hongmd/caddy-pink)
[![License](https://img.shields.io/badge/license-GPL--3.0-blue)](LICENSE)

Custom Docker build that extends the official Caddy image with a set of opinionated modules for Cloudflare-friendly deployments.

## Included modules
- `github.com/caddy-dns/cloudflare` for Cloudflare-managed DNS challenges
- `github.com/WeidiDeng/caddy-cloudflare-ip` to keep up-to-date Cloudflare IP ranges
- `github.com/mholt/caddy-dynamicdns` for pushing records to supported dynamic DNS providers
- `github.com/mholt/caddy-ratelimit` to throttle traffic per route
- `github.com/mholt/caddy-l4` for TCP/UDP (layer 4) proxying and routing
- `github.com/hslatman/caddy-crowdsec-bouncer` to integrate with CrowdSec decisions (HTTP, AppSec, layer 4)
- `github.com/porech/caddy-maxmind-geolocation` to filter requests by MaxMind GeoIP data
- `github.com/ueffel/caddy-tls-format` to log TLS version and cipher suite in readable form

## Building the image
```bash
# from the repository root
cd caddy-pink
docker build -t my-caddy .
```

The upstream Caddy release is pinned by `ARG CADDY_VERSION` at the top of [`caddy-pink/Dockerfile`](caddy-pink/Dockerfile). Override it at build time:
```bash
docker build --build-arg CADDY_VERSION=<version> -t my-caddy .
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

## Examples
See the [`examples/`](examples/) directory for ready-to-use Caddyfile configurations demonstrating each included module. The [Validate examples](.github/workflows/validate-examples.yml) workflow runs `caddy adapt` on each file against the modules in this build, so a broken example fails CI.

## Pulling the published image
Images are published to both registries for every release, in `amd64` and `arm64`:
```bash
docker pull hongmd/caddy-pink:<version>
# or pin a digest, which is the only immutable reference
docker pull hongmd/caddy-pink@sha256:<digest>
```
Each release's notes list the full image digest. Tags `<major>.<minor>`, `<major>` and `latest` track the newest build and are moved on every publish.

## Modifying the build
To add or remove modules, edit the `xcaddy build` statement in `caddy-pink/Dockerfile` and rebuild the image. Refer to the [xcaddy documentation](https://caddyserver.com/docs/build) for syntax details.

## License
Distributed under the terms of the GNU General Public License v3.0. See `LICENSE` for details.
