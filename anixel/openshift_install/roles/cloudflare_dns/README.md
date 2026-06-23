# cloudflare_dns

Cloudflare DNS for OpenShift install endpoints (API, `*.apps`, ingress, etc.). Scaffold only.

## Requirements

- Collection **`community.general`** (Cloudflare modules).
- Cloudflare API token with **Zone:DNS:Edit** on the target zone.

## Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `cloudflare_dns_api_token` | `""` | **Required.** Cloudflare API token. |
| `cloudflare_dns_zone` | `""` | **Required.** DNS zone name (e.g. `example.com`). |
| `cloudflare_dns_base_domain` | `""` | Base domain for cluster FQDNs. |
| `cloudflare_dns_cluster_name` | `""` | Cluster name segment (e.g. `test`). |
| `cloudflare_dns_target_ip` | `""` | Public IP for A records (hypervisor / LB). |

## Description

Creates a wildcard A record `*.{cluster_name}` in the zone (e.g. `*.test` → `*.test.example.com` when `cloudflare_dns_cluster_name: test` and zone `example.com`). Extend `cloudflare_dns_a_record_names` before the role runs to add more record names to the same loop.
