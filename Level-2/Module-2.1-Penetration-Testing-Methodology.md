# 📚 Module 2.1: Penetration Testing Methodology

> **Duration:** Week 1-2 (6-8 hours)  
> **Prerequisites:** Level 1 Complete  
> **Difficulty:** 🟡 Intermediate

## 🎯 Learning Objectives

By the end of this module, you will be able to:
- Understand penetration testing frameworks and methodologies
- Differentiate between vulnerability assessment and penetration testing
- Follow a structured approach to ethical hacking
- Set up a proper testing laboratory environment
- Document findings professionally
- Understand legal and contractual requirements

---

## 🏗️ What is Penetration Testing?

### Definition
**Penetration Testing** (or "pentesting") is a simulated cyberattack against a computer system to check for exploitable vulnerabilities. It's performed to evaluate the security of the system.

### 🎯 Objectives of Penetration Testing
1. **Identify vulnerabilities** before malicious actors do
2. **Test security controls** and their effectiveness
3. **Assess business risk** from security weaknesses
4. **Validate compliance** with security standards
5. **Improve security posture** through actionable recommendations

### 📊 Types of Penetration Testing

| Type | Tester Knowledge | Simulation | Use Case |
|------|------------------|------------|----------|
| **⚫ Black Box** | No internal knowledge | External attacker | Realistic attack simulation |
| **⚪ White Box** | Full internal knowledge | Insider threat/code review | Comprehensive security audit |
| **🩶 Gray Box** | Limited internal knowledge | Contractor/partner access | Balanced approach |

---

## 🎯 Penetration Testing vs Vulnerability Assessment

### 🔍 Vulnerability Assessment
- **Purpose:** Find and catalog vulnerabilities
- **Approach:** Automated scanning with manual verification
- **Depth:** Surface-level identification
- **Output:** List of vulnerabilities with risk ratings
- **Timeline:** Days to weeks

### ⚔️ Penetration Testing
- **Purpose:** Exploit vulnerabilities to demonstrate impact
- **Approach:** Manual testing with targeted exploitation
- **Depth:** Deep dive into specific vulnerabilities
- **Output:** Proof of concept exploits and business impact
- **Timeline:** Weeks to months

### 📈 The Relationship
```
Vulnerability Assessment → Penetration Testing → Red Team Exercise
     (Find Issues)      →    (Exploit Issues)  →   (Simulate APT)
```

---

## 📋 Penetration Testing Frameworks

### 🏛️ PTES (Penetration Testing Execution Standard)

#### **Phase 1: Pre-Engagement Interactions**
- **Scope definition** - What systems/applications to test
- **Rules of engagement** - Authorized techniques and timing
- **Emergency contacts** - Who to call if something goes wrong
- **Legal documentation** - Contracts and liability

#### **Phase 2: Intelligence Gathering**
- **OSINT** (Open Source Intelligence) collection
- **Network reconnaissance** - Discover live hosts and services
- **Application enumeration** - Identify web apps and APIs
- **Social engineering** preparation (if authorized)

#### **Phase 3: Threat Modeling**
- **Attack surface analysis** - Identify potential entry points
- **Threat actor profiling** - Who might attack and how
- **Attack tree creation** - Map possible attack paths
- **Business impact assessment** - Prioritize high-value targets

#### **Phase 4: Vulnerability Analysis**
- **Automated scanning** - Use tools to find known vulnerabilities
- **Manual testing** - Look for logic flaws and misconfigurations
- **Configuration review** - Check security settings
- **Code analysis** (if applicable) - Review source code

#### **Phase 5: Exploitation**
- **Proof of concept** - Demonstrate vulnerabilities are exploitable
- **Privilege escalation** - Gain higher-level access
- **Persistence** - Maintain access (in controlled manner)
- **Data extraction** - Show what data could be compromised

#### **Phase 6: Post-Exploitation**
- **Network pivoting** - Move laterally through systems
- **Data collection** - Document what was accessed
- **Cleanup** - Remove tools and restore systems
- **Documentation** - Record all activities and findings

#### **Phase 7: Reporting**
- **Executive summary** - High-level findings for management
- **Technical details** - Step-by-step reproduction guides
- **Risk assessment** - Business impact and likelihood
- **Remediation** - Specific recommendations to fix issues

### 🎖️ OWASP Testing Guide

The **Open Web Application Security Project (OWASP)** provides comprehensive web application testing methodology:

#### **Information Gathering**
- Conduct search engine discovery and reconnaissance
- Fingerprint web server and applications
- Review webserver metafiles for information leakage
- Enumerate applications on webserver

#### **Configuration and Deployment Management Testing**
- Test network/infrastructure configuration
- Test application platform configuration  
- Test file extensions handling for sensitive information
- Review old, backup and unreferenced files

#### **Identity Management Testing**
- Test role definitions
- Test user registration process
- Test account provisioning process
- Test account enumeration and guessable user accounts

#### **Authentication Testing**
- Test for credentials transported over encrypted channel
- Test for default credentials
- Test for weak lock out mechanism
- Test for bypassing authentication schema

#### **Authorization Testing**
- Test directory traversal/file include
- Test for bypassing authorization schema
- Test for privilege escalation
- Test for insecure direct object references

---

## 🔧 Setting Up Your Penetration Testing Lab

### 💻 Hardware Requirements

**Minimum Setup:**
- **Host Machine:** 16GB RAM, 500GB storage, Intel i5 or equivalent
- **Virtualization:** VirtualBox or VMware
- **Network:** Isolated virtual networks

**Professional Setup:**
- **Dedicated Hardware:** 32GB+ RAM, 1TB+ SSD storage
- **Multiple NICs:** For network segregation
- **USB WiFi Adapter:** For wireless testing
- **Backup Storage:** External drive for lab snapshots

### 🏗️ Lab Architecture

```
┌─────────────────────────────────────────────────┐
│                Host Machine                     │
├─────────────────────────────────────────────────┤
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │
│  │  Kali Linux │  │ Metasploitable │  │ Windows VM  │ │
│  │  (Attacker) │  │   (Target)     │  │  (Target)   │ │
│  └─────────────┘  └─────────────┘  └─────────────┘ │
│         │               │               │         │
├─────────┼───────────────┼───────────────┼─────────┤
│      ┌──▼──────────────▼───────────────▼──┐      │
│      │     Virtual Network (Isolated)     │      │
│      └────────────────────────────────────┘      │
└─────────────────────────────────────────────────┘
```

### 📥 Essential VM Downloads

#### **🛡️ Kali Linux (Attacker Machine)**
```powershell
# Download from official site
# https://www.kali.org/get-kali/
# Choose: "Virtual Machines" → "VMware" or "VirtualBox"
# Size: ~3GB download, ~20GB when extracted
```

#### **🎯 Vulnerable Targets (Practice Machines)**

**Metasploitable 2:**
```powershell
# Download: https://sourceforge.net/projects/metasploitable/
# Purpose: Network service vulnerabilities
# OS: Ubuntu Linux with intentional vulnerabilities
# Default login: msfadmin:msfadmin
```

**DVWA (Damn Vulnerable Web Application):**
```powershell
# Download: https://dvwa.co.uk/
# Purpose: Web application vulnerabilities
# Setup: LAMP/WAMP stack required
# Access: Web browser interface
```

**Windows VMs:**
```powershell
# Download: https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/
# Purpose: Windows-specific testing
# Note: 90-day evaluation versions
# Renew: Take snapshots before expiration
```

**Additional Targets:**
- **WebGoat** - OWASP's web app training platform
- **bWAPP** - Buggy Web Application
- **VulnHub VMs** - Community-created vulnerable machines
- **HackTheBox Retired Machines** - Professional practice targets

### ⚙️ Lab Configuration

#### **Step 1: Network Isolation**
```bash
# Create isolated virtual network in VirtualBox
VBoxManage natnetwork add --netname pentest-lab --network "192.168.100.0/24" --dhcp off

# Or in VMware, create custom network:
# Edit → Virtual Network Editor → Add Network (VMnet2)
# Subnet: 192.168.100.0/24
# No NAT, Host-only or Internal only
```

#### **Step 2: VM Network Configuration**
```bash
# Kali Linux - Configure as attacker
ip addr add 192.168.100.10/24 dev eth0
ip route add default via 192.168.100.1

# Metasploitable - Configure as target
# (Usually gets DHCP automatically)
# Default: 192.168.100.x/24
```

#### **Step 3: Snapshot Creation**
```bash
# Take clean snapshots before testing
VBoxManage snapshot "Kali-Linux" take "Clean-Install" --description "Fresh installation"
VBoxManage snapshot "Metasploitable" take "Clean-Install" --description "Fresh installation"
```

### 🔧 Essential Tool Installation

#### **Kali Linux Additional Tools**
```bash
# Update system
sudo apt update && sudo apt full-upgrade -y

# Install additional tools
sudo apt install -y \
    gobuster \
    dirb \
    nikto \
    sqlmap \
    wpscan \
    enum4linux \
    smbclient \
    onesixtyone \
    snmp-mibs-downloader

# Install Burp Suite Professional (optional)
# Download from: https://portswigger.net/burp
```

#### **Host Machine Tools**
```powershell
# Windows host additional tools
winget install WiresharkFoundation.Wireshark
winget install Nmap.Nmap  
winget install PuTTY.PuTTY
winget install Git.Git
winget install Microsoft.VisualStudioCode

# Browser extensions for testing
# - FoxyProxy (proxy management)
# - Wappalyzer (technology detection)
# - Cookie Editor
# - User-Agent Switcher
```

---

## 📝 Documentation and Reporting

### 📋 Documentation During Testing

#### **Daily Testing Log Template**
```markdown
# Penetration Test Log - [Date]

## Target Information
- **Target:** [IP/Domain]
- **Scope:** [Authorized systems]
- **Start Time:** [HH:MM]
- **End Time:** [HH:MM]

## Activities Performed
### [Time] - [Activity Name]
**Command/Tool Used:**
```bash
nmap -sV -sC target_ip
```

**Results:**
- Finding 1: Description
- Finding 2: Description

**Screenshots:** 
- [Screenshot filename]

**Notes:**
- Important observations
- Next steps to investigate

## Vulnerabilities Discovered
### [Vulnerability Name]
- **Severity:** High/Medium/Low
- **CVSS Score:** X.X
- **Location:** [URL/Service/Port]
- **Description:** [What is vulnerable]
- **Impact:** [What could happen]
- **Proof of Concept:** [How to reproduce]
- **Remediation:** [How to fix]

## Next Session Planning
- [ ] Follow up on interesting findings
- [ ] Test additional attack vectors
- [ ] Research specific vulnerabilities found
```

#### **Screenshot Standards**
- **Naming Convention:** `YYYY-MM-DD_HH-MM_TargetIP_Finding.png`
- **Content Requirements:** 
  - Show command executed
  - Show results clearly
  - Include timestamps
  - Highlight important information
- **Storage:** Organized by target and date

### 📊 Professional Reporting

#### **Executive Summary Template**
```markdown
# Executive Summary

## Assessment Overview
**Client:** [Organization Name]
**Assessment Type:** [Black/Gray/White Box Penetration Test]
**Testing Period:** [Start Date] - [End Date]
**Scope:** [Systems/Applications tested]

## Key Findings
- **Critical Issues:** X vulnerabilities found
- **High Risk Issues:** X vulnerabilities found  
- **Medium Risk Issues:** X vulnerabilities found
- **Low Risk Issues:** X vulnerabilities found

## Business Risk Summary
The assessment identified several security vulnerabilities that could allow 
an attacker to [describe potential business impact]. The most critical 
finding allows [specific impact description].

## Recommendations Priority
1. **Immediate (Critical):** [Top priority fixes]
2. **Short-term (High):** [Address within 30 days]
3. **Medium-term (Medium):** [Address within 90 days]
4. **Long-term (Low):** [Address within 6 months]

## Conclusion
[Overall security posture assessment and recommendations]
```

#### **Technical Details Template**
```markdown
# Technical Findings

## Finding 1: [Vulnerability Name]

**Risk Rating:** Critical/High/Medium/Low
**CVSS 3.1 Score:** X.X
**CWE Reference:** CWE-XXX

### Description
[Detailed technical description of the vulnerability]

### Location
- **Host:** 192.168.1.100
- **Port/Service:** 80/HTTP
- **URL:** https://target.com/vulnerable-page

### Impact
[Detailed explanation of what an attacker could achieve]

### Evidence
```bash
# Commands used to discover
nmap -sV target_ip

# Commands used to exploit
curl -X POST "http://target/login" -d "admin'OR'1'='1"
```

**Screenshots:**
![Finding Screenshot](screenshots/finding1.png)

### Reproduction Steps
1. Navigate to http://target.com/login
2. Enter username: admin
3. Enter password: ' OR '1'='1
4. Observe successful authentication bypass

### Remediation
**Immediate Actions:**
1. [Specific fix instructions]
2. [Configuration changes needed]

**Long-term Recommendations:**
1. [Architectural improvements]
2. [Security controls to implement]

**References:**
- [OWASP Guide Link]
- [Vendor Security Advisory]
- [CVE Reference]
```

---

## ⚖️ Legal and Contractual Considerations

### 📜 Pre-Engagement Requirements

#### **Statement of Work (SOW)**
Essential elements to include:
- **Scope Definition:** Specific systems, IP ranges, applications
- **Testing Types:** Network, web app, wireless, social engineering
- **Authorized Techniques:** What methods are approved
- **Prohibited Actions:** What must NOT be done
- **Timeline:** Start/end dates, testing windows
- **Emergency Procedures:** Contact info and escalation process
- **Deliverables:** Reports, presentations, remediation support

#### **Rules of Engagement (ROE)**
```markdown
# Sample Rules of Engagement

## Authorized Testing Window
- **Dates:** [Start] to [End]
- **Times:** Monday-Friday, 9 AM - 5 PM EST
- **Blackout Periods:** [Holidays, maintenance windows]

## Authorized Techniques
✅ Network scanning and enumeration
✅ Web application testing
✅ Exploitation of discovered vulnerabilities
✅ Privilege escalation attempts
✅ Limited data extraction for proof of concept

## Prohibited Actions
❌ Denial of Service (DoS) attacks
❌ Data destruction or modification
❌ Social engineering against employees
❌ Physical security testing
❌ Testing production databases
❌ Password brute force attacks

## Emergency Contacts
- **Primary:** [Name, Phone, Email]
- **Technical:** [Name, Phone, Email]
- **Legal:** [Name, Phone, Email]

## Reporting Requirements
- **Daily Status:** Brief email updates
- **Critical Findings:** Immediate notification
- **Final Report:** Within 5 business days of completion
```

### 🛡️ Liability and Insurance

#### **Professional Liability**
- **Errors & Omissions Insurance:** Covers mistakes in testing
- **Cyber Liability Insurance:** Covers data breaches during testing  
- **General Liability:** Covers property damage
- **Contract Limitations:** Liability caps and exclusions

#### **Data Handling Requirements**
- **Confidentiality Agreement:** Protect client information
- **Data Classification:** Handle sensitive data appropriately
- **Storage Requirements:** Encrypted storage and transmission
- **Retention Policies:** How long to keep test data
- **Destruction Requirements:** Secure deletion after project

---

## 🔧 Practical Exercise: Lab Setup and First Scan

### **Week 1-2 Challenge: Build Your Penetration Testing Lab**

#### **Phase 1: Environment Setup (4 hours)**

**Step 1: Download and Install VMs**
- [ ] Download VirtualBox or VMware
- [ ] Download Kali Linux VM
- [ ] Download Metasploitable 2
- [ ] Download at least one Windows VM

**Step 2: Network Configuration**
- [ ] Create isolated virtual network
- [ ] Configure VM network settings
- [ ] Test connectivity between VMs
- [ ] Document IP addresses

**Step 3: Tool Verification**
```bash
# Test essential tools in Kali Linux
nmap --version
wireshark --version
burpsuite &
msfconsole
```

**Step 4: Snapshot Creation**
- [ ] Take clean snapshots of all VMs
- [ ] Document snapshot names and purposes
- [ ] Test snapshot restore functionality

#### **Phase 2: First Penetration Test (4 hours)**

**Step 1: Reconnaissance**
```bash
# Discover live hosts
nmap -sn 192.168.100.0/24

# Basic port scan of target
nmap -sV -sC target_ip

# OS fingerprinting
nmap -O target_ip
```

**Step 2: Service Enumeration**
```bash
# Detailed scan of interesting ports
nmap -p 21,22,23,25,53,80,443,993,995 -sV target_ip

# Banner grabbing
nc target_ip 80
telnet target_ip 21
```

**Step 3: Vulnerability Identification**
```bash
# Basic Nmap vulnerability scan
nmap --script vuln target_ip

# Web server scanning (if applicable)
nikto -h http://target_ip
```

**Step 4: Documentation**
Create a test report including:
- [ ] Network topology diagram
- [ ] Host discovery results
- [ ] Service enumeration findings
- [ ] Identified vulnerabilities
- [ ] Screenshots of key findings
- [ ] Next steps for further testing

### **📝 Deliverable: Lab Setup Report**

Create a comprehensive lab documentation including:

```markdown
# Penetration Testing Lab Setup Report

## Environment Overview
**Hypervisor:** [VirtualBox/VMware version]
**Host OS:** [Windows/Linux/macOS version]
**Network Architecture:** [Diagram or description]

## Virtual Machines
### Attacker Machine
- **OS:** Kali Linux [version]
- **IP:** 192.168.100.10
- **RAM:** 4GB
- **Storage:** 40GB
- **Tools Installed:** [List key tools]

### Target Machines
#### Metasploitable 2
- **OS:** Ubuntu Linux [version]
- **IP:** 192.168.100.100
- **Vulnerabilities:** [Known vulnerable services]

[Repeat for other targets]

## Initial Reconnaissance Results
### Network Discovery
[Results of network scans]

### Service Enumeration  
[Detailed service identification]

### Vulnerability Summary
[High-level findings from initial scans]

## Lab Validation
- [ ] All VMs boot successfully
- [ ] Network connectivity confirmed
- [ ] Essential tools operational
- [ ] Snapshots created and tested
- [ ] Documentation complete

## Next Steps
1. [Planned testing activities]
2. [Tools to explore]
3. [Skills to develop]
```

---

## ✅ Module 2.1 Assessment

### **Knowledge Check:**
- [ ] Understands penetration testing methodology frameworks
- [ ] Can differentiate between vulnerability assessment and penetration testing
- [ ] Has set up a functional penetration testing lab
- [ ] Knows legal and contractual requirements
- [ ] Can perform basic reconnaissance and documentation
- [ ] Completed lab setup and first scan exercise

### **Practical Skills Verification:**
- [ ] Successfully built isolated testing environment
- [ ] Performed network discovery and enumeration
- [ ] Documented findings professionally
- [ ] Demonstrated understanding of testing scope and limitations

### **Self-Assessment Questions:**
1. What are the seven phases of the PTES framework?
2. Why is it important to have rules of engagement before testing?
3. What's the difference between black box and white box testing?
4. What should you do if you discover a critical vulnerability during testing?
5. How do you ensure your testing lab is properly isolated?

### **Next Steps:**
✅ **Ready for Module 2.2: Information Gathering**

---

## 🎯 Module 2.1 Complete!

**Congratulations!** You've established the foundation for professional penetration testing. You now understand:
- Penetration testing methodologies and frameworks
- The difference between vulnerability assessment and penetration testing
- How to set up a proper testing laboratory
- Legal and contractual requirements for ethical hacking
- Basic reconnaissance techniques and documentation

**⏭️ Next:** [Module 2.2: Information Gathering](Module-2.2-Information-Gathering.md)

---

*Remember: A methodology is only as good as its implementation. Follow the framework, document everything, and always stay within authorized scope!*
