# 🚩 CTF-Cheatsheets

> A curated collection of hands-on enumeration and exploitation cheatsheets for Capture The Flag (CTF) competitions and penetration testing.

---

## 📂 What's Inside

| Cheatsheet | Description | Topics Covered |
|------------|-------------|----------------|
| [🐧 Linux Enumeration](https://github.com/ArmanAbr/CTF-Cheatsheets/blob/main/Linux-Enumeration/README.md) | Post-exploitation & privilege escalation on Linux systems | System info, users, network, SUID/SGID, sudo, cron jobs, kernel exploits, container escape |
| [🪟 Windows Enumeration](https://github.com/ArmanAbr/CTF-Cheatsheets/blob/main/Windows-Enumeration/README.md) | Post-exploitation & privilege escalation on Windows systems | System info, users, registry, services, scheduled tasks, PowerShell, WMI, PowerUp/WinPEAS |
| [🌐 Web Application Enumeration](https://github.com/ArmanAbr/CTF-Cheatsheets/blob/main/Web-Enumeration/README.md) | Reconnaissance & vulnerability assessment of web apps | Subdomain enum, directory brute-forcing, SQLi, XSS, LFI/RFI, SSRF, SSTI, API testing, CMS |

---

## 🚀 Quick Start

1. **Clone the repository**
   ```bash
   git clone https://github.com/yourusername/CTF-Cheatsheets.git
   cd CTF-Cheatsheets
   ```

2. **Pick your target platform** and open the relevant cheatsheet:
   - Compromised a Linux box? → `linux_enumeration.md`
   - On a Windows machine? → `windows_enumeration.md`
   - Facing a web application? → `web_enumeration.md`

3. **Use the one-liners** at the bottom of each cheatsheet for rapid initial recon.
---

## 🛠️ Recommended Tools

These tools are referenced throughout the cheatsheets. Make sure you have them installed:

| Tool | Purpose | Install |
|------|---------|---------|
| `nmap` | Network scanning | `apt install nmap` |
| `gobuster` / `feroxbuster` | Directory brute-forcing | `apt install gobuster` / `cargo install feroxbuster` |
| `ffuf` | Fast web fuzzer | `apt install ffuf` |
| `sqlmap` | SQL injection automation | `apt install sqlmap` |
| `nikto` | Web vulnerability scanner | `apt install nikto` |
| `nuclei` | Fast vulnerability scanner | [ProjectDiscovery/nuclei](https://github.com/projectdiscovery/nuclei) |
| `subfinder` / `amass` | Subdomain enumeration | [ProjectDiscovery/subfinder](https://github.com/projectdiscovery/subfinder) / [OWASP/Amass](https://github.com/owasp-amass/amass) |
| `PowerUp.ps1` / `WinPEAS` | Windows privilege escalation | [PowerSploit](https://github.com/PowerShellMafia/PowerSploit) / [PEASS-ng](https://github.com/carlospolop/PEASS-ng) |
| `linpeas.sh` | Linux privilege escalation | [PEASS-ng](https://github.com/carlospolop/PEASS-ng) |
| `Burp Suite` / `ZAP` | Web proxy & testing | [portswigger.net](https://portswigger.net/burp) / [zaproxy.org](https://www.zaproxy.org/) |

---

## 💡 General CTF Tips

- **Enumeration is everything.** Spend more time enumerating than exploiting.
- **Read the source.** Always check page source, JS files, and comments.
- **Try the obvious first.** Default credentials, `admin:admin`, `test:test`, etc.
- **Check all vectors.** A web app might have a vulnerable API endpoint, a hidden parameter, or a backup file.
- **Keep notes.** Document what you've tried — CTFs often have rabbit holes.
- **Use the one-liners.** Each cheatsheet ends with quick one-liners for rapid assessment.
