# ip.bzh

**What is my IP address?** A Breton web service, minimal and privacy-respecting, to find out your public IP address and diagnose your connection. Made in Brittany, hosted by [Ti Nuage](https://ti-nuage.fr).

🌐 **[ip.bzh](https://ip.bzh)**

```console
$ curl ip.bzh
203.0.113.7
```

## Highlights

- **Simple**: the page shows your address, nothing else in the way.
- **IPv6 coming soon**: IPv6 support is on its way.
- **No trace**: no access logs, no stored addresses. Only aggregated counters are kept.
- **Zero dependencies**: Go standard library only, a single static binary with templates, translations and assets embedded.
- **Multilingual**: French, Breton, English, German, Spanish, Italian.

## Command-line usage

```console
$ curl ip.bzh                     # address only, plain text
$ curl ip.bzh/json                # address, country, city, ISP, ASN, hostname
$ curl 'ip.bzh/json?ip=1.1.1.1'   # information about another address
$ curl ip.bzh/port/443            # open, closed or filtered
$ nc ip.bzh 23                    # raw TCP reply (BusyBox routers, minimal containers)
```

## What the page shows

- Your public IP address
- Country (flag) and city
- Internet service provider (ISP) and AS number
- Hostname (reverse DNS)

Geolocation relies on local [DB-IP Lite](https://db-ip.com/db/lite.php) databases (CC BY 4.0) read directly by the server: **no call to any third-party service**.

## Toolbox

The **Tools** tab includes:

| Tool | Description |
|---|---|
| DNS leak test | Which resolvers actually answer for you? The server is itself the authoritative DNS of a dedicated zone |
| WebRTC leak test | Compares the address seen by WebRTC (embedded STUN server) with your connection's address |
| Open port test | Tests a TCP port on *your own* address, never a third party's |
| Ping / Traceroute | Run from the server, to public IPv4 targets only |
| IP lookup | Country, city, ISP and ASN of an address, from the local databases |
| DNS query | A, AAAA, MX, NS, TXT, CNAME, PTR, never returning non-public addresses |
| CIDR calculator | IPv4 and IPv6, entirely in the browser |
| IPv6 generator | ULA (RFC 4193) and prefix splitting into /56, /60, /64, in the browser |

## Privacy

- No access logs (nginx `access_log off`); no address is logged or persisted.
- `/stats` only exposes per-day aggregated counters: endpoint, client type, IP family, language.
- Audience measurement via self-hosted Matomo, cookieless, Do Not Track honored.

## Security

**Reporting a vulnerability**: `contact@ti-nuage.fr` (published in [`/.well-known/security.txt`](https://ip.bzh/.well-known/security.txt), RFC 9116).

## Contributing

Translations are welcome, especially **Breton proofreading by native speakers**: contact `contact@ti-nuage.fr`.
