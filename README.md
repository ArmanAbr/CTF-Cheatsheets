# CTF-Cheatsheets

> A curated collection of hands-on enumeration and exploitation cheatsheets for Capture The Flag (CTF) competitions and penetration testing.

---

## What's Inside

| Cheatsheet | Description | Topics Covered |
|------------|-------------|----------------|
| [Linux Enumeration](https://github.com/ArmanAbr/CTF-Cheatsheets/blob/main/Linux-Enumeration/README.md) | Post-exploitation & privilege escalation on Linux systems | System info, users, network, SUID/SGID, sudo, cron jobs, kernel exploits, container escape |
| [Windows Enumeration](https://github.com/ArmanAbr/CTF-Cheatsheets/blob/main/Windows-Enumeration/README.md) | Post-exploitation & privilege escalation on Windows systems | System info, users, registry, services, scheduled tasks, PowerShell, WMI, PowerUp/WinPEAS |
| [Web Application Enumeration](https://github.com/ArmanAbr/CTF-Cheatsheets/blob/main/Web-Enumeration/README.md) | Reconnaissance & vulnerability assessment of web apps | Subdomain enum, directory brute-forcing, SQLi, XSS, LFI/RFI, SSRF, SSTI, API testing, CMS |
| [SMB Enumeration](https://github.com/ArmanAbr/CTF-Cheatsheets/blob/main/SMB-Enumeration/README.md) | Enumerating and attacking SMB services | Share enum, user enum, RPC, password attacks, NTLM relay, Pass-the-Hash, EternalBlue |

---

## Recommended Tools

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
| `impacket` | SMB/NTLM/Python network protocols | [fortra/impacket](https://github.com/fortra/impacket) |
| `crackmapexec` | SMB pentesting swiss army knife | [Porchetta-Industries/CrackMapExec](https://github.com/Porchetta-Industries/CrackMapExec) |
| `responder` | LLMNR/NBT-NS/mDNS poisoner | [lgandx/Responder](https://github.com/lgandx/Responder) |
| `enum4linux` | SMB enumeration | `apt install enum4linux` |
| `smbmap` / `smbclient` | SMB share access & listing | `apt install smbclient` |

---
