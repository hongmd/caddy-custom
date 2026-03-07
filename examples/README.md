# Example Caddyfiles

Ready-to-use configuration examples for every module included in
**caddy-pink**. Copy a file into your project, adjust the placeholders
(`example.com`, tokens, upstreams), and you're good to go.

| Example | Modules demonstrated |
|---------|---------------------|
| [cloudflare-dns.Caddyfile](cloudflare-dns.Caddyfile) | `caddy-dns/cloudflare`, `caddy-cloudflare-ip` |
| [dynamic-dns.Caddyfile](dynamic-dns.Caddyfile) | `caddy-dynamicdns`, `caddy-dns/cloudflare` |
| [rate-limit.Caddyfile](rate-limit.Caddyfile) | `caddy-ratelimit` |
| [crowdsec.Caddyfile](crowdsec.Caddyfile) | `caddy-crowdsec-bouncer` (HTTP + L4) |
| [full-stack.Caddyfile](full-stack.Caddyfile) | All modules combined |

## Prerequisites

Some modules require external credentials passed as environment variables:

- **`CF_API_TOKEN`** — Cloudflare API token with `Zone:DNS:Edit` permission
- **`CROWDSEC_API_KEY`** — CrowdSec bouncer API key (from `cscli bouncers add`)
- **`MAXMIND_DB_PATH`** — Path to the MaxMind GeoLite2-City database file
