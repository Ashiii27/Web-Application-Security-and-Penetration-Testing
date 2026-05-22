## 1. Nmap — Network & Port Scanner

### How It Works
Nmap (Network Mapper) sends **specially crafted packets** to a target host and analyzes the responses to determine:
- Which **ports** are open, closed, or filtered
- What **services** are running on those ports
- What **version** of those services are running
- What **operating system** the target is likely running

```
You (Nmap)                        Target Host
    │                                  │
    │── SYN packet ──────────────────▶ │  Port 80
    │◀─ SYN-ACK (open) ────────────── │  → Port is OPEN
    │                                  │
    │── SYN packet ──────────────────▶ │  Port 23
    │◀─ RST (closed) ──────────────── │  → Port is CLOSED
    │                                  │
    │── SYN packet ──────────────────▶ │  Port 8080
    │   [no response / ICMP blocked]   │  → Port is FILTERED
```

### Installation
```bash
# Kali / Debian / Ubuntu
sudo apt install nmap -y

# Verify
nmap --version
```

### Basic Usage

```bash
# Scan a single host (top 1000 ports)
nmap 192.168.1.1

# Scan a domain
nmap example.com

# Scan multiple hosts
nmap 192.168.1.1 192.168.1.2 192.168.1.3

# Scan a whole subnet
nmap 192.168.1.0/24
```

### Important Flags Explained

| Flag | Full Name | What It Does | Example |
|------|-----------|-------------|---------|
| `-sS` | SYN Scan | Stealthy half-open scan (default, needs root) | `nmap -sS target` |
| `-sT` | TCP Connect | Full TCP scan (no root needed) | `nmap -sT target` |
| `-sU` | UDP Scan | Scans UDP ports (slower) | `nmap -sU target` |
| `-sV` | Version Detection | Detects service versions | `nmap -sV target` |
| `-O` | OS Detection | Tries to identify the OS | `nmap -O target` |
| `-A` | Aggressive | Runs -sV, -O, scripts, traceroute | `nmap -A target` |
| `-p` | Ports | Specify ports to scan | `nmap -p 80,443 target` |
| `-p-` | All Ports | Scan all 65535 ports | `nmap -p- target` |
| `-T4` | Timing | Faster scan (T0=paranoid → T5=insane) | `nmap -T4 target` |
| `-oN` | Output Normal | Save output to a text file | `nmap -oN out.txt target` |
| `-oA` | Output All | Save in all formats | `nmap -oA results target` |
| `-v` | Verbose | Show more detail during scan | `nmap -v target` |
| `--open` | Open Only | Show only open ports | `nmap --open target` |
| `-iL` | Input List | Read targets from a file | `nmap -iL hosts.txt` |
| `--script` | NSE Scripts | Run Nmap Scripting Engine scripts | `nmap --script=http-title target` |

### Practical Commands

```bash
# Quick scan — find what's alive on a subnet
nmap -sn 192.168.1.0/24

# Service version detection on common ports
nmap -sV -p 21,22,23,25,53,80,110,143,443,3306,3389 target.com

# Full aggressive scan with output saved
nmap -A -p- -T4 -oA full_scan target.com

# Scan for common web ports only
nmap -p 80,443,8080,8443,8000,8888 target.com

# Use NSE scripts for HTTP info
nmap --script=http-title,http-headers,http-methods target.com

# Check for vulnerabilities
nmap --script=vuln target.com

# OS + version detection
nmap -sV -O target.com
```

### Understanding Nmap Output

```
PORT     STATE    SERVICE    VERSION
22/tcp   open     ssh        OpenSSH 7.4 (protocol 2.0)
80/tcp   open     http       Apache httpd 2.4.29
443/tcp  open     ssl/https  nginx 1.14.0
3306/tcp closed   mysql
8080/tcp filtered http-proxy
```

- **open** → Port is accepting connections. A service is running.
- **closed** → Port is reachable but no service is listening.
- **filtered** → Firewall or ACL is blocking Nmap's probe packets.

### Use Cases
- **Reconnaissance:** Find all open ports on a target before deeper testing
- **Asset Discovery:** Map all live hosts on a network segment
- **Service Identification:** Know exactly what software and version is running
- **Vulnerability Assessment:** Use `--script=vuln` to find known CVEs

---
