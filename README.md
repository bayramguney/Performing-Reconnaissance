# Performing-Reconnaissance

# Assisted Lab: Performing Reconnaissance

## Overview

This lab demonstrates the **reconnaissance phase of a penetration test**, where security professionals collect information about a target organization before performing vulnerability assessments or security testing.

As a security team member of **Structureality Inc**, this exercise focuses on performing:

- OSINT (Open-Source Intelligence) gathering
- Domain information discovery
- DNS enumeration
- Whois reconnaissance
- Google Dorking
- Website intelligence gathering
- Public record research

The goal of reconnaissance is to understand the target's attack surface, identify publicly available information, and gather intelligence that can help security teams improve defenses.

---

# Lab Environment

## Virtual Machines

| System | Purpose |
|---|---|
| Kali Linux | Reconnaissance and security testing workstation |
| Simulated Internet Environment | Structureality target environment |

## Tools Used

- Kali Linux Terminal
- Ping
- Whois
- Nslookup
- Dig
- Google Search Operators
- Netcraft
- SearchSystems OSINT Database

---

# Security+ Objectives Covered

This lab aligns with the following **CompTIA Security+ SY0-701 objectives**:

### 4.3 - Vulnerability Management
- Understand reconnaissance activities used during vulnerability assessments
- Identify exposed services and information

### 5.3 - Third-Party Risk Assessment and Management
- Gather intelligence about external organizations and services

### 5.5 - Types and Purposes of Audits and Assessments
- Understand penetration testing activities and information gathering techniques

---

# Lab Tasks and Findings

# 1. Identify Kali Linux Network Information

The first step in reconnaissance is understanding the attacker's own network position.

Command used:

```bash
ip a s eth0
```

Purpose:

- Identify the Kali Linux IP address
- Verify network connectivity
- Confirm communication with the simulated internet environment

---

# 2. Website Discovery and Connectivity Testing

Target:

```
www.structureality.com
```

Command used:

```bash
ping www.structureality.com -c 4 > target_info.txt
```

The output was saved to a file for documentation.

View results:

```bash
cat target_info.txt
```

## Findings

Target website resolved to:

```
203.0.113.1
```

Observation:

- DNS resolution was successful
- Ping requests were sent
- No ICMP responses were returned

Possible reasons:

- Firewall protection
- ICMP disabled
- Network security configuration

Security Insight:

> Lack of ping response does not always indicate that a host is unavailable. Many organizations block ICMP traffic as a security measure.

---

# 3. Website OSINT Gathering

The Structureality website was accessed through Firefox.

Information discovered:

- Company name
- Business address
- Phone number
- Email contact information

During a real penetration test, this information would be documented because it may reveal:

- Employee information
- Contact points
- Social engineering opportunities
- Attack surface details

---

# 4. Whois Reconnaissance

Whois provides registration information about a domain.

Command used:

```bash
whois -h 192.0.2.10 structureality.com > target_whois.txt
```

View results:

```bash
cat target_whois.txt
```

## Findings

Registrar:

```
515support
```

Whois information can reveal:

- Domain ownership
- Registrar information
- Administrative contacts
- Organization details

Security Importance:

Attackers can use Whois information during reconnaissance, while defenders can monitor and minimize unnecessary exposure.

---

# 5. DNS Reconnaissance

DNS enumeration helps identify:

- IP addresses
- Mail servers
- Name servers
- Domain relationships

## Using Nslookup

Start interactive mode:

```bash
nslookup
```

Check DNS server:

```bash
server
```

Target lookup:

```
www.structureality.com
```

Finding:

```
203.0.113.1
```

---

# DNS Record Enumeration

## Name Server (NS)

Command:

```
set type=ns
structureality.com
```

Finding:

```
ns.structureality.com
```

IP Address:

```
203.0.113.225
```

---

## Mail Exchange (MX)

Command:

```
set type=mx
structureality.com
```

Finding:

```
mail.structureality.com
```

---

## Mail Server A Record

Command:

```
set type=a
mail.structureality.com
```

Finding:

```
203.0.113.1
```

---

## Canonical Name (CNAME)

Command:

```
set type=cname
website.structureality.com
```

Finding:

```
website.structureality.com
--> www.structureality.com
```

---

## Start of Authority (SOA)

Command:

```
set type=soa
structureality.com
```

Information discovered:

- Primary DNS server
- DNS administrator email
- Zone serial number
- Refresh interval
- Retry interval

---

# 6. DNS Enumeration Using Dig

The dig command was used because it provides better output capture for penetration testing reports.

Command:

```bash
dig @203.0.113.225 structureality.com > target_dns.txt
```

Additional records were appended:

```bash
dig @203.0.113.225 www.structureality.com >> target_dns.txt
```

```bash
dig @203.0.113.225 structureality.com -t mx >> target_dns.txt
```

```bash
dig @203.0.113.225 structureality.com -t ns >> target_dns.txt
```

Review:

```bash
cat target_dns.txt
```

Benefits:

- Creates evidence
- Supports penetration testing documentation
- Provides historical reference

---

# 7. Google Dorking

Google Dorking uses advanced search operators to locate publicly available information.

Examples:

## Finding robots.txt

Search:

```
site:twitter.com filetype:txt robots
```

Purpose:

- Discover indexed files
- Identify restricted directories
- Understand search engine exposure

---

## Searching Password Related Pages

Example:

```
intitle:password site:linkedin.com
```

Purpose:

Identify pages containing password-related information.

---

## Finding Cisco Configuration Files

Search:

```
filetype:cfg "enable password 7"
```

Finding:

Cisco configuration files containing weak password hashes.

Example hash:

```
09424F0A170414425D
```

Security Lesson:

Cisco Type 7 passwords are weak and can be cracked quickly.

Modern systems should use stronger authentication methods.

---

# 8. Netcraft OSINT Investigation

Tool:

Netcraft Site Report

Website:

```
https://sitereport.netcraft.com/
```

Target analyzed:

```
comptia.org
```

Information gathered:

- Hosting information
- Network details
- Historical information
- Domain relationships
- Subdomains

Security Value:

Netcraft demonstrates how attackers and defenders can collect publicly available intelligence about websites.

---

# 9. Background Research Using Public Records

Tool:

SearchSystems

Purpose:

Explore publicly available government databases.

Available information categories included:

- Court records
- Licenses
- Marriage and divorce records
- Property ownership
- Tax records

Security Importance:

Public records can provide information useful for:

- Identity verification
- Social engineering awareness
- Privacy assessments

---

# Key Security Concepts Learned

## Reconnaissance

The process of gathering information about a target before security testing.

Types:

### Passive Reconnaissance

Collecting publicly available information without directly interacting with the target.

Examples:

- Whois
- Google Dorking
- Netcraft
- Public records

### Active Reconnaissance

Directly interacting with the target.

Examples:

- Ping
- DNS queries
- Service discovery

---

# Attack Surface Information Discovered

During this lab, the following information was identified:

| Information Type | Example |
|---|---|
| Domain | structureality.com |
| Web Server IP | 203.0.113.1 |
| DNS Server | ns.structureality.com |
| Mail Server | mail.structureality.com |
| Registrar | 515support |
| DNS Records | A, MX, NS, SOA, CNAME |

---

# Defensive Recommendations

Organizations should:

✅ Minimize unnecessary public information exposure  
✅ Regularly review DNS records  
✅ Remove outdated records and services  
✅ Protect employee information  
✅ Monitor for leaked credentials  
✅ Disable insecure password storage methods  
✅ Perform regular security assessments  

---

# Skills Demonstrated

- OSINT investigation
- DNS enumeration
- Whois analysis
- Network troubleshooting
- Google search operators
- Security documentation
- Penetration testing methodology
- Reconnaissance reporting

---

# Conclusion

This lab provided practical experience with the reconnaissance phase of penetration testing. By using tools such as Whois, Nslookup, Dig, Google Dorking, Netcraft, and public record databases, security professionals can identify exposed information and better understand an organization's attack surface.

Reconnaissance is a critical first step in cybersecurity because understanding what information is publicly available helps organizations reduce risk and strengthen their security posture.
