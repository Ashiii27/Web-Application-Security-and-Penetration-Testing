## 5. WhatWeb — Web Technology Fingerprinter

### How It Works
**WhatWeb** sends HTTP requests to a target web server and analyzes the response — headers, HTML content, cookies, scripts, and URLs — to identify:
- Web server software (Apache, Nginx, IIS)
- Programming language and frameworks (PHP, Ruby on Rails, Django)
- CMS platforms (WordPress, Joomla, Drupal)
- JavaScript libraries (jQuery, React, Angular)
- Security headers (or lack thereof)
- Analytics and marketing tools

```
WhatWeb sends HTTP Request
        │
        ▼
   Analyzes Response:
   ├── Headers  (Server: Apache/2.4.29)
   ├── Cookies  (PHPSESSID → PHP)
   ├── HTML     (<meta generator="WordPress 5.9">)
   ├── Scripts  (/wp-includes/js/jquery.js → WordPress)
   └── URLs     (/wp-admin/ → WordPress admin path)
        │
        ▼
   Identified Technologies
```

### Installation
```bash
# Kali (pre-installed)
whatweb --version

# Ubuntu / Debian
sudo apt install whatweb -y

# From source
git clone https://github.com/urbanadventurer/WhatWeb.git
cd WhatWeb && gem install bundler && bundle install
```

### Basic Usage

```bash
# Basic scan
whatweb https://example.com

# Verbose output (shows what matched each detection)
whatweb -v https://example.com

# Scan multiple targets
whatweb https://example.com https://test.example.com

# Scan from a file
whatweb -i targets.txt

# Follow redirects
whatweb -F https://example.com

# Save output to file
whatweb https://example.com -o results.txt
whatweb https://example.com --log-json results.json
```

### Aggression Levels

WhatWeb supports **aggression levels** that control how actively it probes the target:

```bash
# Level 1 (default) — stealthy, single request
whatweb -a 1 https://example.com

# Level 2 — slightly more requests, check common paths
whatweb -a 2 https://example.com

# Level 3 — aggressive, checks many paths and plugins
whatweb -a 3 https://example.com

# Level 4 — very aggressive, sends many requests
whatweb -a 4 https://example.com
```

### Important Flags

| Flag | What It Does | Example |
|------|-------------|---------|
| `-v` | Verbose — show matched patterns | `whatweb -v target.com` |
| `-a` | Aggression level (1-4) | `whatweb -a 3 target.com` |
| `-i` | Read targets from file | `whatweb -i hosts.txt` |
| `-o` | Save output to file | `whatweb -o out.txt target.com` |
| `--log-json` | Save output as JSON | `whatweb --log-json out.json target.com` |
| `-F` | Follow HTTP redirects | `whatweb -F target.com` |
| `--no-errors` | Suppress error messages | `whatweb --no-errors target.com` |
| `--color` | Control colored output | `whatweb --color=never target.com` |
| `-q` | Quiet mode | `whatweb -q target.com` |

### Reading the Output

```
http://example.com [200 OK]
  Apache[2.4.29]          ← Web server and version
  Bootstrap[3.3.7]        ← CSS framework version
  Country[UNITED STATES]  ← Geographic location
  HTML5                   ← HTML version
  HTTPServer[Apache/2.4.29 (Ubuntu)]
  IP[93.184.216.34]
  JQuery[3.2.1]           ← JavaScript library version
  PHP[7.2.1]              ← Server-side language and version
  Title[Example Domain]   ← Page title
  WordPress[5.9.3]        ← CMS and version → check for CVEs!
  X-Powered-By[PHP/7.2.1]
```

### Use Cases
- Identify the **exact CMS and version** to look up known CVEs
- Find **outdated software** that may be vulnerable
- Discover the **technology stack** to tailor your attack approach
- Check for **missing security headers** (X-Frame-Options, CSP, etc.)
- Quickly scan a **list of subdomains** to map their tech stacks

---
