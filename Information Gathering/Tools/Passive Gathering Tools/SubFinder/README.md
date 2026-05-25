## 8. Subfinder — Passive Subdomain Discovery

### How It Works
**Subfinder** is a fast, passive subdomain enumeration tool. Unlike Gobuster (which brute-forces), Subfinder queries **50+ passive sources** — certificate transparency logs, public DNS datasets, threat intelligence feeds, and search engines — without sending a single packet to the target.

```
Subfinder
  │
  ├── crt.sh           → SSL certificate subdomains
  ├── SecurityTrails   → DNS history
  ├── VirusTotal       → Threat intelligence
  ├── Shodan           → Internet-wide scans
  ├── Censys           → Certificate & port data
  ├── dnsdumpster      → DNS datasets
  ├── HackerTarget     → DNS datasets
  ├── Riddler          → DNS passive data
  └── 40+ more sources...
       │
       ▼
  All results → deduplicated → sorted
```

### Installation
```bash
# Via Go (recommended)
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest

# Via apt (older version on Kali)
sudo apt install subfinder -y

# Verify
subfinder -version
```

### Configuring API Keys (Improves Results)
```bash
# Create config file
mkdir -p ~/.config/subfinder
nano ~/.config/subfinder/provider-config.yaml

# Add your keys:
# securitytrails:
#   - YOUR_API_KEY
# shodan:
#   - YOUR_API_KEY
# virustotal:
#   - YOUR_API_KEY
# censys:
#   - YOUR_API_ID
#   - YOUR_API_SECRET
```

### Basic Usage

```bash
# Basic passive scan
subfinder -d example.com

# Save output to file
subfinder -d example.com -o subdomains.txt

# Quiet mode (only subdomains, no banner)
subfinder -d example.com -silent

# Scan multiple domains from file
subfinder -dL domains.txt -o all_subs.txt

# Show source that found each subdomain
subfinder -d example.com -v

# Use only specific sources
subfinder -d example.com -sources shodan,censys,crtsh
```

### Important Flags

| Flag | What It Does | Example |
|------|-------------|---------|
| `-d` | Target domain | `-d example.com` |
| `-dL` | File with list of domains | `-dL domains.txt` |
| `-o` | Output file | `-o subs.txt` |
| `-oJ` | Output as JSON | `-oJ subs.json` |
| `-silent` | Only show results | `-silent` |
| `-v` | Verbose — show sources | `-v` |
| `-t` | Concurrent goroutines (threads) | `-t 100` |
| `-timeout` | Timeout per source (seconds) | `-timeout 30` |
| `-sources` | Use only these sources | `-sources shodan,crtsh` |
| `-all` | Use all sources (slower) | `-all` |
| `-recursive` | Enable recursive subdomain enum | `-recursive` |

### Chaining with Other Tools

```bash
# Find subs → probe live ones with httpx
subfinder -d example.com -silent | httpx -silent -status-code -title

# Find subs → port scan with nmap
subfinder -d example.com -silent -o subs.txt
nmap -iL subs.txt -p 80,443 --open -oN open_web.txt

# Find subs → check for takeover with nuclei
subfinder -d example.com -silent | nuclei -t takeovers/

# Multi-tool pipeline
subfinder -d example.com -silent | \
  httpx -silent | \
  tee live_hosts.txt
```

### Use Cases
- Discover **all subdomains** without touching the target
- Find **forgotten staging/dev environments** (dev.example.com, old.example.com)
- Identify the **full scope** before starting an engagement
- Chain with **httpx** to quickly find live web services
- Use as input for **vulnerability scanners** like Nuclei

---
