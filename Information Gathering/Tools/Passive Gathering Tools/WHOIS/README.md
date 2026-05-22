## 2. Whois — Domain Registration Lookup

### How It Works
**Whois** queries the WHOIS protocol — a public database maintained by ICANN and regional registrars. When a domain is registered, the registrant's information is stored in this database. Whois fetches and displays this registration record.

```
Your Terminal                   WHOIS Server
     │                               │
     │── whois example.com ────────▶ │  Queries IANA → Registrar DB
     │◀─ Registration Record ─────── │
     │                               │
     Displays: registrant, dates,
               name servers, contacts
```

### Installation
```bash
# Usually pre-installed on Linux
sudo apt install whois -y

# Verify
whois --version
```

### Basic Usage

```bash
# Basic domain lookup
whois example.com

# IP address lookup
whois 93.184.216.34

# Lookup with specific WHOIS server
whois -h whois.arin.net 93.184.216.34
```

### Important Flags

| Flag | What It Does | Example |
|------|-------------|---------|
| `-h` | Specify WHOIS server | `whois -h whois.arin.net 8.8.8.8` |
| `-p` | Use specific port | `whois -p 43 example.com` |
| `-r` | Disable recursion | `whois -r example.com` |

### Filtering Useful Information

```bash
# Show only registrar info
whois example.com | grep -i registrar

# Show only name servers
whois example.com | grep -i "name server"

# Show only dates
whois example.com | grep -i "date\|expir\|creat\|updat"

# Show registrant details
whois example.com | grep -i "registrant\|admin\|tech"

# Show contact emails
whois example.com | grep -i "email"

# Get all key info in one shot
whois example.com | grep -iE "registrar|registrant|name server|email|created|expir"
```

### Sample Output Explained

```
Domain Name: EXAMPLE.COM                    ← The domain
Registrar: IANA                             ← Who manages the registration
Created Date: 1992-01-01T00:00:00Z          ← Original registration date
Updated Date: 2023-08-14T07:01:31Z          ← Last modification
Registry Expiry Date: 2024-08-13T04:00:00Z  ← When it expires (takeover risk!)
Name Server: A.IANA-SERVERS.NET             ← DNS servers (target for zone transfer)
Name Server: B.IANA-SERVERS.NET
Registrant Email: noc@iana.org              ← Contact email (useful for OSINT)
```

### Use Cases
- Find **who owns** a domain and their contact info
- Reveal **name servers** to attempt DNS zone transfers
- Identify **domain expiry dates** (expired domains can be taken over)
- Discover the **hosting provider** and ISP
- Build a contact map for social engineering research
- Find related domains registered to the same email

---
