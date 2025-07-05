# 📚 Module 2.2: Information Gathering

> **Duration:** Week 3-4 (6-8 hours)  
> **Prerequisites:** Module 2.1 Complete  
> **Difficulty:** 🟡 Intermediate

## 🎯 Learning Objectives

By the end of this module, you will be able to:
- Perform comprehensive passive reconnaissance using OSINT techniques
- Conduct active reconnaissance safely and effectively
- Use network discovery and enumeration tools
- Gather information about target organizations and infrastructure
- Understand the reconnaissance kill chain
- Document intelligence gathering results systematically

---

## 🔍 Introduction to Information Gathering

### The Reconnaissance Kill Chain

Information gathering is the foundation of any successful penetration test. It follows a systematic approach:

```
📡 Passive Reconnaissance → 🎯 Active Reconnaissance → 📊 Analysis & Planning
     (OSINT Collection)    →  (Direct Interaction)   →  (Attack Strategy)
```

### 🎭 Types of Reconnaissance

#### **🔇 Passive Reconnaissance**
- **Definition:** Collecting information without directly interacting with the target
- **Advantage:** Completely undetectable
- **Sources:** Public records, social media, search engines, DNS records
- **Risk Level:** Zero - no direct contact with target systems

#### **🔊 Active Reconnaissance**
- **Definition:** Directly interacting with target systems to gather information
- **Advantage:** More detailed and current information
- **Sources:** Network scanning, service enumeration, banner grabbing
- **Risk Level:** Moderate - may be detected by security systems

---

## 🌐 Passive Reconnaissance (OSINT)

### 🔍 Search Engine Intelligence

#### **Google Dorking Techniques**

**Basic Google Operators:**
```bash
# Find specific file types
site:target.com filetype:pdf
site:target.com filetype:xlsx
site:target.com filetype:docx

# Find login pages
site:target.com inurl:login
site:target.com inurl:admin
site:target.com intitle:"login"

# Find error messages and debug info
site:target.com "error" OR "exception" OR "debug"
site:target.com "mysql error" OR "sql error"

# Find directory listings
site:target.com intitle:"index of"

# Find configuration files
site:target.com filetype:conf OR filetype:config
site:target.com filetype:env OR filetype:ini

# Find email addresses
site:target.com "@target.com" -www

# Find subdomains
site:*.target.com -www
```

**Advanced Google Dorking:**
```bash
# Cached pages (useful if site is down)
cache:target.com

# Similar sites
related:target.com

# Exclude certain results
site:target.com -www -mail

# Find specific content in URL
site:target.com inurl:admin intitle:login

# Social media mentions
"target.com" site:linkedin.com OR site:twitter.com OR site:facebook.com

# Find exposed databases
"target.com" intitle:"phpMyAdmin" OR intitle:"Database"

# Find webcams or IoT devices
"target.com" inurl:"view.shtml" OR inurl:"ViewerFrame"
```

#### **Other Search Engines**
```bash
# Bing specific operators
site:target.com ip:192.168.1.1

# DuckDuckGo (privacy-focused)
site:target.com !google

# Yandex (often finds different results)
site:target.com

# Shodan (IoT and service discovery)
org:"Target Organization"
net:"192.168.1.0/24"
```

### 🏢 Corporate Intelligence

#### **Company Information Sources**

**Public Business Records:**
- **SEC Filings** (for US public companies)
- **Companies House** (UK company records)
- **Corporate websites** and investor relations
- **Annual reports** and financial statements
- **Press releases** and news articles

**Professional Networks:**
```bash
# LinkedIn reconnaissance
# - Employee enumeration
# - Organizational structure
# - Technology stack (job postings)
# - Company connections

# GitHub/GitLab scanning
# - Organization repositories
# - Employee personal repos
# - Potential secret leaks
# - Technology preferences
```

#### **OSINT Framework Tools**

**Website Analysis:**
- **Wayback Machine** (archive.org) - Historical website versions
- **BuiltWith** - Technology stack identification
- **Wappalyzer** - Web technology profiler
- **Netcraft** - Website technology and hosting info

**Social Media Intelligence:**
- **Social-Searcher** - Cross-platform social media search
- **Maltego** - Link analysis and data visualization
- **theHarvester** - Email and subdomain discovery
- **Sherlock** - Username enumeration across platforms

### 🌍 DNS Intelligence

#### **DNS Enumeration Techniques**

**Basic DNS Queries:**
```bash
# Basic DNS lookup
nslookup target.com
dig target.com

# Find mail servers
dig target.com MX
nslookup -type=MX target.com

# Find name servers
dig target.com NS
nslookup -type=NS target.com

# Find all DNS records
dig target.com ANY
nslookup -type=ANY target.com

# Reverse DNS lookup
dig -x 192.168.1.1
nslookup 192.168.1.1
```

**Advanced DNS Techniques:**
```bash
# Zone transfer attempts (often restricted)
dig @nameserver target.com AXFR
host -t AXFR target.com nameserver

# DNS cache snooping
dig @dns-server target.com +norecurse

# Find CNAMEs and aliases
dig target.com CNAME

# TXT record enumeration (often contains useful info)
dig target.com TXT
```

#### **Subdomain Discovery**

**Manual Techniques:**
```bash
# Common subdomain wordlist
www.target.com
mail.target.com
ftp.target.com
admin.target.com
test.target.com
dev.target.com
staging.target.com
api.target.com
blog.target.com
shop.target.com
```

**Automated Tools:**
```bash
# Sublist3r - Python subdomain discovery tool
sublist3r -d target.com -t 50

# Amass - comprehensive subdomain enumeration
amass enum -d target.com

# DNSrecon - DNS reconnaissance tool
dnsrecon -d target.com -t brt

# Gobuster - directory/subdomain brute forcer
gobuster dns -d target.com -w /usr/share/wordlists/subdomains.txt

# Fierce - DNS scanner
fierce -dns target.com
```

### 📧 Email and Personnel Intelligence

#### **Email Harvesting**

**theHarvester Tool:**
```bash
# Search multiple sources for emails
theHarvester -d target.com -l 100 -b google
theHarvester -d target.com -l 100 -b linkedin
theHarvester -d target.com -l 100 -b bing
theHarvester -d target.com -l 100 -b all

# Save results to file
theHarvester -d target.com -l 100 -b all -f results.html
```

**Manual Email Discovery:**
```bash
# Common email patterns
firstname.lastname@target.com
first.last@target.com
flast@target.com
firstnamelastname@target.com

# Department emails
admin@target.com
support@target.com
sales@target.com
info@target.com
contact@target.com
```

#### **Social Media Reconnaissance**

**LinkedIn Intelligence:**
- Employee enumeration and titles
- Organizational structure mapping
- Technology skills and certifications
- Recent hires and departures
- Company connections and partnerships

**Professional Information Sources:**
```bash
# Search for company employees
site:linkedin.com "Target Company"
site:linkedin.com "works at Target Company"

# Find technical staff
site:linkedin.com "Target Company" "security"
site:linkedin.com "Target Company" "IT" OR "administrator"
site:linkedin.com "Target Company" "developer" OR "programmer"
```

---

## 🎯 Active Reconnaissance

### 🌐 Network Discovery

#### **Host Discovery Techniques**

**Ping Sweeps:**
```bash
# ICMP ping sweep
nmap -sn 192.168.1.0/24

# TCP SYN ping (useful when ICMP is blocked)
nmap -PS80,443,22 192.168.1.0/24

# UDP ping
nmap -PU 192.168.1.0/24

# ARP ping (local network only)
nmap -PR 192.168.1.0/24

# Skip ping and assume hosts are up
nmap -Pn 192.168.1.0/24
```

**Custom Scripts:**
```bash
#!/bin/bash
# Simple ping sweep script
for ip in 192.168.1.{1..254}; do
    ping -c 1 -W 1 $ip | grep "64 bytes" | cut -d" " -f4 | cut -d":" -f1 &
done
wait
```

#### **Port Scanning Strategies**

**Nmap Scanning Techniques:**
```bash
# TCP SYN scan (default, fast and stealthy)
nmap -sS target.com

# TCP Connect scan (more reliable but easier to detect)
nmap -sT target.com

# UDP scan (slower but important for UDP services)
nmap -sU target.com

# Comprehensive scan
nmap -sS -sU -O -sV -sC target.com

# Intense scan with version detection
nmap -T4 -A -v target.com

# Scan top 1000 ports quickly
nmap --top-ports 1000 target.com

# Custom port range
nmap -p 1-65535 target.com
nmap -p 80,443,22,21,25,53,110,143,993,995 target.com
```

**Timing and Stealth:**
```bash
# Timing templates (T0-T5)
nmap -T0 target.com  # Paranoid (very slow)
nmap -T1 target.com  # Sneaky (slow)
nmap -T2 target.com  # Polite (slow)
nmap -T3 target.com  # Normal (default)
nmap -T4 target.com  # Aggressive (fast)
nmap -T5 target.com  # Insane (very fast)

# Fragmentation and decoys
nmap -f target.com                    # Fragment packets
nmap -D RND:10 target.com            # Use random decoys
nmap -S spoofed_ip target.com        # Spoof source IP
```

### 🔍 Service Enumeration

#### **Banner Grabbing**

**Manual Banner Grabbing:**
```bash
# HTTP banner grabbing
nc target.com 80
HEAD / HTTP/1.0

# HTTPS banner grabbing
openssl s_client -connect target.com:443
HEAD / HTTP/1.0

# SSH banner grabbing
nc target.com 22

# FTP banner grabbing
nc target.com 21

# Telnet banner grabbing
telnet target.com 23

# SMTP banner grabbing
nc target.com 25
HELO test.com
```

**Automated Banner Grabbing:**
```bash
# Nmap service version detection
nmap -sV target.com

# Nmap script scanning
nmap -sC target.com
nmap --script banner target.com

# Amap - application mapper
amap -A target.com 1-1000

# Netcat banner grab script
for port in 21 22 23 25 53 80 110 143 443 993 995; do
    echo "Banner for port $port:"
    timeout 5 nc target.com $port < /dev/null
    echo "---"
done
```

#### **Service-Specific Enumeration**

**HTTP/HTTPS Services:**
```bash
# HTTP enumeration with dirb
dirb http://target.com

# HTTP enumeration with gobuster
gobuster dir -u http://target.com -w /usr/share/wordlists/dirb/common.txt

# Nikto web scanner
nikto -h http://target.com

# Whatweb - web technology identifier
whatweb target.com

# HTTP methods testing
nmap --script http-methods target.com

# SSL/TLS testing
nmap --script ssl-enum-ciphers target.com
sslscan target.com
```

**SMB/NetBIOS Services:**
```bash
# SMB enumeration
enum4linux target.com

# SMB shares enumeration
smbclient -L //target.com -N
smbmap -H target.com

# NetBIOS enumeration
nbtscan target.com
nmap --script smb-enum-shares target.com
nmap --script smb-enum-users target.com
```

**DNS Services:**
```bash
# DNS enumeration
dnsrecon -d target.com
dnsenum target.com

# DNS zone transfer
dig @target.com target.com AXFR
host -t AXFR target.com target.com
```

**SNMP Services:**
```bash
# SNMP enumeration
snmpwalk -c public -v1 target.com
onesixtyone -c community_strings.txt target.com

# SNMP Nmap scripts
nmap --script snmp-info target.com
nmap --script snmp-brute target.com
```

### 🕷️ Web Application Reconnaissance

#### **Technology Stack Identification**

**Manual Techniques:**
```bash
# HTTP headers analysis
curl -I http://target.com

# Technology detection
whatweb http://target.com

# Wappalyzer (browser extension)
# - Server technologies
# - Programming languages
# - Frameworks and libraries
# - Analytics tools
```

**Automated Tools:**
```bash
# BuiltWith API
# - Technology profiling
# - Historical data
# - Competitor analysis

# Retire.js
# - JavaScript library vulnerabilities
# - Outdated component detection
```

#### **Directory and File Discovery**

**Wordlist-Based Discovery:**
```bash
# Dirb with common wordlist
dirb http://target.com /usr/share/wordlists/dirb/common.txt

# Gobuster directory discovery
gobuster dir -u http://target.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt

# Dirbuster GUI tool
# - Comprehensive directory discovery
# - File extension testing
# - Custom wordlists

# FFuF - fast web fuzzer
ffuf -u http://target.com/FUZZ -w /usr/share/wordlists/dirb/common.txt
```

**Custom Discovery:**
```bash
# Common files and directories
robots.txt
sitemap.xml
.htaccess
.git/
.svn/
backup/
admin/
test/
dev/
staging/
api/
```

---

## 🛠️ Essential Reconnaissance Tools

### 🔧 Passive Reconnaissance Tools

#### **theHarvester**
```bash
# Install
sudo apt install theharvester

# Basic usage
theHarvester -d target.com -l 100 -b google

# Multiple sources
theHarvester -d target.com -l 200 -b google,bing,linkedin,twitter

# Output to file
theHarvester -d target.com -l 100 -b all -f results.xml
```

#### **Maltego**
```bash
# Community Edition (free)
# - Visual link analysis
# - OSINT data aggregation
# - Transform-based investigation
# - Social network mapping

# Professional features
# - Advanced transforms
# - Larger datasets
# - Collaboration tools
```

#### **Recon-ng**
```bash
# Install and setup
sudo apt install recon-ng
recon-ng

# Basic commands
[recon-ng][default] > workspaces create target_company
[recon-ng][target_company] > modules search
[recon-ng][target_company] > modules load discovery/info_disclosure/interesting_files
[recon-ng][target_company] > info
[recon-ng][target_company] > options set SOURCE target.com
[recon-ng][target_company] > run
```

### 🎯 Active Reconnaissance Tools

#### **Nmap (Network Mapper)**
```bash
# Advanced Nmap techniques
nmap -sS -A -T4 -oA full_scan target.com

# Script categories
nmap --script-help "*"
nmap --script discovery target.com
nmap --script vuln target.com
nmap --script auth target.com

# Custom NSE scripts
nmap --script /path/to/custom-script.nse target.com
```

#### **Masscan**
```bash
# Ultra-fast port scanner
masscan -p1-65535 192.168.1.0/24 --rate=1000

# Output to file
masscan -p80,443 192.168.1.0/24 --rate=10000 -oG results.txt

# Banner grabbing with masscan
masscan -p80,443 192.168.1.0/24 --banners --rate=1000
```

#### **Zmap**
```bash
# Internet-wide scanning (use responsibly!)
zmap -p 80 -o results.txt

# Target specific networks
echo "192.168.1.0/24" | zmap -p 80

# Rate limiting
zmap -p 80 -r 1000 192.168.1.0/24
```

---

## 📊 Information Analysis and Documentation

### 🗂️ Data Organization

#### **Information Categories**
```markdown
# Target Organization Intelligence

## Corporate Information
- Company name and aliases
- Business registration details
- Physical addresses
- Phone numbers and contact info
- Key personnel and executives
- Business partnerships
- Financial information

## Technical Infrastructure
- Domain names and subdomains
- IP address ranges
- Hosting providers
- Technology stack
- Email servers
- DNS servers

## Personnel Information
- Employee names and titles
- Email addresses
- Social media profiles
- Professional certifications
- Technical skills and expertise

## Public Exposure
- Publicly accessible services
- Open ports and protocols
- Web applications
- File shares and repositories
- IoT devices and webcams
```

#### **Documentation Templates**

**Reconnaissance Report Template:**
```markdown
# Reconnaissance Report - [Target Organization]

## Executive Summary
**Target:** [Organization Name]
**Scope:** [Authorized domains/IP ranges]
**Methodology:** [OSINT + Active Reconnaissance]
**Key Findings:** [High-level summary]

## Corporate Intelligence
### Business Information
- **Legal Name:** [Full company name]
- **DBA/Aliases:** [Alternative names]
- **Industry:** [Business sector]
- **Size:** [Employee count estimate]
- **Locations:** [Physical addresses]

### Key Personnel
| Name | Title | Contact Info | Social Media |
|------|-------|--------------|--------------|
| [Name] | [Title] | [Email/Phone] | [LinkedIn, etc.] |

## Technical Infrastructure
### Domain Portfolio
| Domain | Registrar | Creation Date | Expiry Date |
|--------|-----------|---------------|-------------|
| target.com | [Registrar] | [Date] | [Date] |

### Subdomain Discovery
| Subdomain | IP Address | Services | Notes |
|-----------|------------|----------|-------|
| www.target.com | 192.168.1.1 | HTTP/HTTPS | [Comments] |

### IP Range Analysis
| Network | Provider | Location | Usage |
|---------|----------|----------|-------|
| 192.168.1.0/24 | [ISP] | [Location] | [Purpose] |

## Service Enumeration
### Web Applications
| URL | Technology | Version | Notes |
|-----|------------|---------|-------|
| https://target.com | Apache | 2.4.41 | [Observations] |

### Network Services
| IP | Port | Service | Version | Vulnerability |
|----|------|---------|---------|---------------|
| 192.168.1.1 | 22 | SSH | OpenSSH 7.4 | [CVE if applicable] |

## Attack Surface Analysis
### External Exposure
- **Web Applications:** [Count and types]
- **Email Services:** [Mail servers and protocols]
- **Remote Access:** [VPN, RDP, SSH]
- **File Services:** [FTP, SMB, NFS]

### Potential Entry Points
1. **Web Application Vulnerabilities**
2. **Weak Authentication Services** 
3. **Unpatched Network Services**
4. **Social Engineering Vectors**

## Recommendations
### Immediate Actions
1. [Priority recommendations]

### Long-term Improvements  
1. [Strategic recommendations]

## Appendices
### Tool Output
[Raw tool outputs and screenshots]

### Wordlists Used
[Custom wordlists and sources]

### Timeline
[Reconnaissance timeline and milestones]
```

---

## 🔧 Practical Exercise: Complete Reconnaissance

### **Week 3-4 Challenge: Comprehensive Target Reconnaissance**

#### **Phase 1: Passive Reconnaissance (4 hours)**

**Step 1: OSINT Collection**
- [ ] Perform Google dorking on target domain
- [ ] Search for company information and personnel
- [ ] Enumerate subdomains using multiple techniques
- [ ] Collect email addresses and social media profiles
- [ ] Analyze DNS records and historical data

**OSINT Checklist:**
```bash
# Google dorking
site:target.com filetype:pdf
site:target.com intitle:login
site:target.com "error" OR "exception"

# Subdomain discovery
sublist3r -d target.com
amass enum -d target.com

# Email harvesting
theHarvester -d target.com -l 100 -b all

# DNS enumeration
dig target.com ANY
dnsrecon -d target.com
```

**Step 2: Social Media Intelligence**
- [ ] Search LinkedIn for employees
- [ ] Check GitHub for organization repositories
- [ ] Look for technology mentions in job postings
- [ ] Find social media accounts and posts

#### **Phase 2: Active Reconnaissance (4 hours)**

**Step 1: Network Discovery**
```bash
# Host discovery
nmap -sn target_network/24

# Port scanning
nmap -sS -T4 -p- target_ip

# Service enumeration  
nmap -sV -sC target_ip

# UDP scan of common ports
nmap -sU --top-ports 100 target_ip
```

**Step 2: Service-Specific Enumeration**
```bash
# Web services
nikto -h http://target_ip
dirb http://target_ip
gobuster dir -u http://target_ip -w /usr/share/wordlists/dirb/common.txt

# SMB services (if applicable)
enum4linux target_ip
smbclient -L //target_ip -N

# DNS services (if applicable)
dnsrecon -d target.com -n target_ip
dig @target_ip target.com AXFR
```

**Step 3: Web Application Analysis**
```bash
# Technology identification
whatweb http://target_ip

# Directory discovery
gobuster dir -u http://target_ip -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,html,txt

# File discovery
gobuster dir -u http://target_ip -w /usr/share/wordlists/dirb/common.txt -x bak,old,txt,conf
```

### **📝 Deliverable: Comprehensive Reconnaissance Report**

Create a detailed reconnaissance report including:

```markdown
# Comprehensive Reconnaissance Report

## Executive Summary
[High-level findings and recommendations]

## Methodology
[Tools and techniques used]

## OSINT Findings
### Corporate Intelligence
[Company information, personnel, business details]

### Digital Footprint
[Subdomains, email addresses, social media presence]

## Active Reconnaissance Results
### Network Infrastructure
[IP ranges, host discovery, network topology]

### Service Enumeration
[Open ports, running services, technology stack]

### Web Application Assessment
[Directory structure, technology identification, potential attack vectors]

## Risk Assessment
### Attack Surface Analysis
[Exposed services and potential entry points]

### Threat Modeling
[Likely attack scenarios based on findings]

## Recommendations
### Security Improvements
[Specific recommendations to reduce attack surface]

### Monitoring Recommendations
[Suggestions for detecting reconnaissance activities]

## Appendices
[Tool outputs, screenshots, raw data]
```

---

## ✅ Module 2.2 Assessment

### **Knowledge Check:**
- [ ] Understands difference between passive and active reconnaissance
- [ ] Can perform comprehensive OSINT using multiple sources
- [ ] Knows how to use network discovery and enumeration tools
- [ ] Can analyze and document reconnaissance findings
- [ ] Understands attack surface analysis concepts
- [ ] Completed comprehensive reconnaissance exercise

### **Practical Skills Verification:**
- [ ] Successfully gathered intelligence using OSINT techniques
- [ ] Performed network discovery and service enumeration
- [ ] Documented findings in professional format
- [ ] Demonstrated understanding of reconnaissance operational security

### **Self-Assessment Questions:**
1. What are the advantages and disadvantages of passive vs active reconnaissance?
2. How can Google dorking be used to find sensitive information?
3. What information can be gathered from DNS records?
4. How do you minimize detection during active reconnaissance?
5. What should be included in a reconnaissance report?

### **Next Steps:**
✅ **Ready for Module 2.3: Vulnerability Scanning**

---

## 🎯 Module 2.2 Complete!

**Congratulations!** You've mastered the art of information gathering. You now understand:
- Passive reconnaissance techniques and OSINT methodology
- Active reconnaissance and network enumeration
- Service discovery and banner grabbing
- Web application reconnaissance
- Intelligence analysis and documentation

**⏭️ Next:** [Module 2.3: Vulnerability Scanning](Module-2.3-Vulnerability-Scanning.md)

---

*Remember: Information is power in cybersecurity. The more you know about your target, the more effective your testing will be. Always gather intelligence responsibly and within scope!*
