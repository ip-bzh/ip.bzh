# ip.bzh

**What is my IP address?** A Breton web service, minimal and privacy-respecting, to find out your public IP address and diagnose your connection. Made in Brittany, hosted by [Ti Nuage](https://ti-nuage.fr).

🌐 **[ip.bzh](https://ip.bzh)**

```console
$ curl ip.bzh
203.0.113.7
```

## Highlights

- **Simple**: the page shows your address, nothing else in the way. `curl ip.bzh` prints it bare.
- **No trace**: no access logs, no stored addresses. Only aggregated counters are kept.
- **No third-party calls for your address**: geolocation comes from local [DB-IP Lite](https://db-ip.com/db/lite.php) databases (CC BY 4.0) read by the server.
- **Zero dependencies**: Go standard library only. A single static binary holds the templates, translations and assets.
- **Works without JavaScript**: the pages and the tools (through plain forms) work without it; JavaScript adds live results and the browser checks.
- **Multilingual**: French, Breton, English, German, Spanish, Italian.
- **Light and dark themes**: follows the system, or a choice you make.
- **IPv4 and IPv6**: the single-family hosts `4.ip.bzh` and `6.ip.bzh` let the home page show both of your addresses.

## Command-line usage

```console
$ curl ip.bzh                      # your address, plain text
$ wget -qO- ip.bzh                 # the same with wget
$ irm ip.bzh                       # the same in PowerShell
$ curl ip.bzh/ip                   # always plain text, even when HTML is asked for

$ curl ip.bzh/json                 # address, version, country, city, ISP, AS number, hostname
$ curl 'ip.bzh/json?ip=1.1.1.1'    # the same for another address (without hostname)
$ curl 'ip.bzh/json?lang=fr'       # place names in French (also br, de, es, it)

$ curl ip.bzh/country              # one field, plain text:
$ curl ip.bzh/country-code         #   country, ISO 3166 code,
$ curl ip.bzh/city                 #   city,
$ curl ip.bzh/asn                  #   AS number,
$ curl ip.bzh/isp                  #   access provider or host,
$ curl ip.bzh/hostname             #   reverse DNS

$ curl ip.bzh/port/443             # can your port 443 be reached? open, closed or filtered
$ curl ip.bzh/port/22,80,443       # up to 10 ports, one "<port> <state>" line each

$ curl ip.bzh/asn/13335            # every network of an autonomous system, one per line

$ nc ip.bzh 23                     # your address over bare TCP (BusyBox routers, minimal containers)
$ telnet ip.bzh 23                 # the same with telnet

$ curl ip.bzh/stats                # usage statistics, aggregated, JSON
$ curl ip.bzh/help                 # every command, as a manual page (?lang=fr…)
```

## Home page

- Your public IP address, with a *Copy* button
- Country (flag) and city
- Internet service provider (ISP) and AS number
- Hostname (reverse DNS)
- Your address of the other IP family, when you have one
- The kind of network the address belongs to: an access provider's, a host's or VPN provider's, or a Tor exit
- A notice when your address changes while the page is open (a VPN switched on or off, another network)

## Tools

Every result can be copied as text or as a link (`#q=…`, `#ping=…`, `#port=…`, `#cidr=…`…) that runs the tool again when opened.

- **DNS leak test**: which resolvers actually answer for you? The server is itself the authoritative DNS of a dedicated zone. It also tells how they behave: the part of your address they pass on (EDNS Client Subnet), their defences against forged answers (random source ports, 0x20, cookies), QNAME minimisation and DNSSEC.
- **WebRTC leak test**: compares the address WebRTC reveals (through the server's own STUN server) with your connection's address.
- **Open port test**: up to 10 TCP ports of *your own* address, never a third party's.
- **Ping and traceroute**: run from the server, to public IPv4 targets only.
- **DNS propagation**: one question to six public resolvers (Cloudflare, Google, Quad9, DNS4EU, AdGuard, DNS.SB) and to the domain's own servers, to see whether a change has reached everyone.
- **E-mail header analyzer**: sender, SPF/DKIM/DMARC results and the route of a message, server by server, with the time of each step. Read in the browser: the headers are sent nowhere.
- **CIDR calculator** (IPv4 and IPv6) and **IPv6 generator** (ULA prefixes, splitting into /56, /60, /64), entirely in the browser.

### Address or domain lookup

One search field takes an address, a domain, an AS number or a pasted URL.

- **An address**: location and ISP, reverse DNS, its network at the regional registry (RDAP), its BGP route and RPKI state (RIPEstat), and the public blocklists mail servers consult (Spamhaus, SpamCop, PSBL, Mailspike, DroneBL).
- **An AS number** (`AS3215`): its IPv4 and IPv6 networks, from the local database.
- **A domain**:
  - **DNS**: A, AAAA, CNAME, MX, NS, TXT, CAA and HTTPS records, and DNSSEC validation;
  - **Mail**: SPF, DMARC, MTA-STS, TLS-RPT, BIMI and DKIM keys;
  - **Mail servers**: addresses, reverse DNS, blocklists, STARTTLS on port 25 (no mail sent) and DANE (TLSA records against the certificate);
  - **Web site**: certificate, HTTP to HTTPS redirect, HTTP/3, security headers;
  - **Registration**: registrant, registrar, dates, status and abuse contact, over RDAP (whois for the TLDs without RDAP).

## Browser tab

The **Browser** tab shows what any website learns about you without asking, sums up the points to watch with an identification risk, and gives advice for your browser (Firefox, Brave, Chrome/Edge, Safari). The checks only run on request.

- **Received by the server** (without JavaScript): User-Agent, Do Not Track and Global Privacy Control, the connection (HTTP and TLS versions, cipher, post-quantum key exchange, round-trip time) and the headers that describe the browser.
- **Network fingerprint**: the TCP SYN (system and MTU, the way p0f reads it), the TLS ClientHello (JA4) and the start of HTTP/2 (Akamai's fingerprint), read on a second port of the server.
- **Readable by JavaScript**: system and browser versions (Client Hints), languages, time zone, keyboard layout, screen, hardware, graphics card, devices, preferences and permission states.
- **What a site can infer**:
  - leaks: WebRTC, DNS, and the address of the other IP family;
  - VPN, proxy and Tor hints;
  - an inconsistent or spoofed User-Agent, tampering and automation;
  - an ad blocker, private browsing and third-party cookies;
  - anti-fingerprinting protections.
- **Browser fingerprint**: an identifier built from 22 elements, each with its identifying power, plus a cross-browser identifier that links two browsers of one device. It is computed in the browser and never sent; on request, it is kept locally to show what changed at the next visit.
- **Even without JavaScript**: a stylesheet alone tells the server the screen size, theme, engine and some installed fonts.
- **Report**: everything the page shows, as text or JSON.

## Other pages

- **Help** (`/{lang}/help/`): every command, laid out as a manual page.
- **Statistics** (`/{lang}/stats/`): requests per day and what they were for, drawn on the server. The JSON is at `/stats.json`.
- **Privacy policy** and **legal notice**.

## Privacy

- **Your address** is looked up in local databases only, and kept in memory only as long as the rate limits need it.
- **Searches** go to third parties only where the answer needs them: registries over RDAP or whois, blocklists over DNS, RIPEstat for the BGP route of an address, public resolvers for the propagation test, and the domain's own web, mail and DNS servers. Nothing is logged.
- **Statistics**: only per-day aggregated counters (endpoint, client type, IP family, language). Robots are not counted.
- **Audience measurement**: self-hosted Matomo, cookieless. It is not loaded under Do Not Track or Global Privacy Control, and can be refused from the privacy page.

## Security

Reporting a vulnerability: `contact@ip.bzh` (published in [`/.well-known/security.txt`](https://ip.bzh/.well-known/security.txt), RFC 9116).

## Contributing

Translations are welcome, especially **Breton proofreading by native speakers**: contact `contact@ti-nuage.fr`.
