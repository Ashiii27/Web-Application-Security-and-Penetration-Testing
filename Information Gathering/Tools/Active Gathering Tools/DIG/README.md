## 3. Dig — DNS Query Tool

### How It Works
**Dig** (Domain Information Groper) sends DNS queries to DNS servers and displays the full response. Unlike nslookup, dig gives you raw, detailed DNS responses including TTL values, authoritative servers, and all record data.

```
Your Terminal                  DNS Resolver              Authoritative NS
     │                              │                           │
     │── dig example.com ─────────▶ │                           │
     │                              │── Query A record ────────▶ │
     │                              │◀─ IP Address ─────────── │
     │◀─ Full DNS Response ──────── │
     │
     Displays: Answer, Authority,
               Additional sections
```

### Installation
```bash
# Part of dnsutils package
sudo apt install dnsutils -y

# Verify
dig -v
```

### Basic Usage

```bash
# Basic A record lookup
dig example.com

# Short answer only
dig +short example.com

# Query specific record type
dig example.com MX
dig example.com TXT
dig example.com NS
dig example.com AAAA

# Reverse DNS lookup
dig -x 93.184.216.34

# Query a specific DNS server
dig @8.8.8.8 example.com        # Use Google's DNS
dig @1.1.1.1 example.com        # Use Cloudflare's DNS
```

### Important Flags

| Flag | What It Does | Example |
|------|-------------|---------|
| `+short` | Display only the answer | `dig +short example.com` |
| `+noall +answer` | Show only answer section | `dig +noall +answer example.com` |
| `+trace` | Trace DNS resolution path | `dig +trace example.com` |
| `ANY` | Request all record types | `dig example.com ANY` |
| `@server` | Use a specific DNS server | `dig @8.8.8.8 example.com` |
| `-x` | Reverse lookup (PTR) | `dig -x 93.184.216.34` |
| `axfr` | Zone transfer attempt | `dig axfr @ns1.example.com example.com` |
| `+stats` | Show query statistics | `dig +stats example.com` |

### DNS Record Types Explained

```bash
# A Record — IPv4 address mapping
dig example.com A
# Output: example.com. 3600 IN A 93.184.216.34

# AAAA Record — IPv6 address mapping
dig example.com AAAA
# Output: example.com. 3600 IN AAAA 2606:2800:21f:cb07:6820:80da:af6b:8b2c

# MX Record — Mail servers (target for phishing research)
dig example.com MX
# Output: example.com. 3600 IN MX 0 mail.example.com.

# NS Record — Name servers (try zone transfer against these)
dig example.com NS
# Output: example.com. 3600 IN NS a.iana-servers.net.

# TXT Record — SPF, DKIM, verification tokens
dig example.com TXT
# Output: "v=spf1 -all" (reveals email security config)

# CNAME — Canonical name / alias
dig www.example.com CNAME
# Output: www.example.com. 3600 IN CNAME example.com.

# SOA — Start of Authority (admin info)
dig example.com SOA
# Output: primary NS, admin email, serial, refresh times
```

### Zone Transfer Attempt

A **DNS Zone Transfer** (AXFR) copies the entire DNS zone from a server. Misconfigured servers may allow this, revealing all subdomains and internal hostnames.

```bash
# Step 1: Find name servers
dig example.com NS +short

# Step 2: Attempt zone transfer against each NS
dig axfr @ns1.example.com example.com
dig axfr @ns2.example.com example.com

# Successful zone transfer reveals ALL records:
# dev.example.com, admin.example.com, internal.example.com, etc.
```

### Use Cases
- Map the **full DNS structure** of a target
- Discover **mail servers** (MX records) for phishing/email security checks
- Reveal **SPF/DKIM/DMARC** configuration from TXT records
- Attempt **zone transfers** to get all subdomains at once
- Trace the **full DNS resolution path** to understand infrastructure

---
