## 9. Netcat — Banner Grabbing & Connectivity

### How It Works
**Netcat** (nc) is a networking utility that reads and writes data across TCP/UDP connections. For information gathering, it's used for **banner grabbing** — connecting to open ports and reading the service banner that identifies the software and version.

```
Netcat                            Target Service
   │                                    │
   │── TCP Connect to port 22 ────────▶ │
   │◀─ SSH-2.0-OpenSSH_7.4p1 ──────── │  ← Banner reveals version!
   │                                    │
   │── TCP Connect to port 80 ────────▶ │
   │── "HEAD / HTTP/1.0\r\n\r\n" ────▶ │
   │◀─ Server: Apache/2.4.29 ──────── │  ← Header reveals version!
```

### Installation
```bash
# Usually pre-installed
nc --version

# Install if missing
sudo apt install netcat-openbsd -y   # Recommended (OpenBSD version)
# OR
sudo apt install ncat -y             # Nmap's version
```

### Banner Grabbing Commands

```bash
# Grab SSH banner
nc -nv 192.168.1.1 22
# Output: SSH-2.0-OpenSSH_7.4p1 Ubuntu-10ubuntu0.1

# Grab HTTP server header
echo -e "HEAD / HTTP/1.0\r\n\r\n" | nc -nv example.com 80
# Output: HTTP/1.1 200 OK
#         Server: Apache/2.4.29 (Ubuntu)
#         X-Powered-By: PHP/7.2.1

# Grab FTP banner
nc -nv 192.168.1.1 21
# Output: 220 ProFTPD 1.3.5e Server (ProFTPD) [192.168.1.1]

# Grab SMTP banner
nc -nv 192.168.1.1 25
# Output: 220 mail.example.com ESMTP Postfix (Ubuntu)

# Grab Telnet banner
nc -nv 192.168.1.1 23
# Output: Welcome to Linux Telnet Server

# Grab MySQL banner
nc -nv 192.168.1.1 3306
# Output: (binary data with MySQL version)
```

### Important Flags

| Flag | What It Does | Example |
|------|-------------|---------|
| `-n` | No DNS resolution (use IPs) | `nc -n 192.168.1.1 80` |
| `-v` | Verbose output | `nc -v example.com 80` |
| `-z` | Zero-I/O mode (port scanning) | `nc -z -v example.com 1-1000` |
| `-w` | Connection timeout (seconds) | `nc -w 3 example.com 80` |
| `-u` | UDP mode | `nc -u example.com 53` |
| `-l` | Listen mode | `nc -l -p 4444` |
| `-p` | Local port number | `nc -p 9000 example.com 80` |

### Quick Port Scan with Netcat

```bash
# Scan a range of ports (basic)
nc -zv 192.168.1.1 1-1000

# Check specific ports
nc -zv 192.168.1.1 22 80 443 3306

# Scan with timeout
nc -zv -w 1 192.168.1.1 1-65535 2>&1 | grep -i open
```

### Use Cases
- **Banner grabbing** to identify service software and version
- **Port checking** when Nmap is unavailable
- Verify **connectivity** to a port before deeper testing
- **Manual HTTP requests** to inspect raw server responses

---
