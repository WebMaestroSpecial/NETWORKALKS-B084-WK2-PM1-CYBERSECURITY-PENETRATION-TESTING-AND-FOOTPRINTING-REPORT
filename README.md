# NETWORKWALKS-B084-WK2-PM1-CYBERSECURITY-PENETRATION-TESTING-AND-FOOTPRINTING-REPORT
The result of penetration testing and footprinting for academic purpose

PENETRATION TESTING & FOOTPRINTING REPORT

Target Domain: networkwalks.com
Assessment Type: Passive Reconnaissance & Footprinting

Date: November 12, 2026

Classification: Confidential
1. Executive Summary
This report documents the initial footprinting and passive reconnaissance phase performed
against networkwalks.com. All security evaluation activities were strictly limited to non-intrusive
information-gathering techniques conducted within the scope defined by the prior authorization
agreement.
The primary objective was to map the target's public exposure, identify perimeter network
infrastructure, determine web server configurations, and evaluate security headers and
protection layers using exact output data captured from standard security analysis tools on Kali
Linux.
2. Scope & Authorization
Authorized Scope
● Target Domain: networkwalks.com
● Permitted Activities: Passive DNS queries, domain registration lookups, web application
fingerprinting, HTTP response header analysis, and Web Application Firewall (WAF)
detection.
Authorization Verification
ℹ️ Authorization Confirmation: Testing was conducted in full compliance with the formal
authorization letter issued by Networkwalks management. All activities were restricted
strictly to passive and low-impact footprinting to prevent any disruption to live services.
3. Methodology & Tools Used
The assessment utilized 6 standard Kali Linux tools:
Tool Purpose / Category Primary Objective
WHOIS Domain Reconnaissance Queries public registries
for domain registration
details, administrative
contacts, and
nameservers.
WAFW00F WAF Detection Identifies and fingerprints
Web Application Firewalls
protecting the host.
NSLookup Name Server Query Queries Internet domain
name servers to resolve
IP addresses and verify
DNS response mapping.
DNSRecon DNS Enumeration Conducts detailed
enumeration of DNS
records (SOA, NS, A, MX,
TXT, SRV).
CURL (curl -I) Header Analysis Fetches HTTP response
headers to evaluate
server signatures,
caching, and active
security policies.
WhatWeb Tech Stack Fingerprinting Identifies underlying
CMS, web servers,
embedded scripts, and
platform versions.
4. Detailed Findings & Evidences
The following subsections present the actual execution results, embedded screenshot artifacts,
and textual output files.
4.1 Web Application Firewall Detection (wafw00f)
● Objective: Determine if the target host is protected by a Web Application Firewall (WAF).
● Evidence Screenshot: wafw00f.png
● Evidence Log: wafw00f_result.txt
Visual Evidence
Log Artifact Output
Plaintext
Key Technical Observations:
* WAFW00F confirmed that networkwalks.com is actively protected by ModSecurity
(SpiderLabs) WAF.
* ModSecurity provides web application-layer protection against common web attacks prior
to traffic reaching application controllers.
4.2 Name Server Resolution (nslookup)
● Objective: Perform initial standard DNS resolution for networkwalks.com.
● Evidence Screenshot: nslookup.png
● Evidence Log: nslookup_result.txt
Visual Evidence
Key Technical Observations:
* Standard DNS query using Google public DNS (8.8.8.8) resolves networkwalks.com directly to
target host IP 192.232.216.135.
4.3 Detailed DNS Record Enumeration (dnsrecon)
● Objective: Retrieve full domain DNS record structures including SOA, NS, MX, TXT, and
SRV records.
● Evidence Screenshot: dnsrecon.jpg
● Evidence Log: dnsrecon_result.txt
Visual Evidence
Key Technical Observations:
* Name Servers: HostGator infrastructure (ns6135.hostgator.com - 50.87.144.87 and
ns6136.hostgator.com - 192.232.216.131).
* DNS Server Version: BIND version 9.16.23-RH.
* Mail Server (MX): mail.networkwalks.com resolving to 192.232.216.135.
* SPF Record: v=spf1 +a +mx +ip4:50.87.144.87 +include:websitewelcome.com ~all.
* SRV Records: cPanel email autodiscover services (cpanelemaildiscovery.cpanel.net).
4.4 HTTP Header Analysis (curl -I)
● Objective: Inspect HTTP response headers to identify web server software and active
security policies.
● Evidence Screenshot: curl_I.png
● Evidence Log: curl_I_result.txt
Visual Evidence
Key Technical Observations:
* Server Header: Apache HTTP Server.
* Caching & Nginx Proxy: Headers X-nginx-cache: WordPress and X-Endurance-Cache-Level:
0 reveal an upstream/reverse Nginx caching layer typically used in HostGator WordPress
hosting environments.
* WordPress Endpoints: Header Link exposes the WordPress REST API endpoint (/wp-json/).
* Cookie Security: Set-Cookie sets __wpdm_client with both secure and HttpOnly flags
properly enabled.
4.5 Web Application Technology Stack Identification (whatweb)
● Objective: Comprehensive technology fingerprinting of target web platform and scripts.
● Evidence Screenshot: whatweb.png
● Evidence Log: whatweb_result.txt
Visual Evidence
Key Technical Observations:
* CMS & Version: WordPress version 7.1.4.
* Active Plugins: WordPress Download Manager version 3.3.76.
* Frontend Frameworks: Bootstrap 7.1.4 and jQuery 3.7.1.
* Exposed Contact Information: Public organization email discovered:
info@networkwalks.com.
* Page Title: Networkwalks Academy.
4.6 Domain Registration Reconnaissance (whois)
● Objective: Obtain public registrar information and administrative contact exposure.
Summary Analysis
* Domain queries verify registration under standard domain privacy protection masking personal
details. Nameservers discovered directly match the active HostGator infrastructure
(ns6135.hostgator.com and ns6136.hostgator.com) identified during dnsrecon enumeration.
5. Security Recommendations & Actionable
Remediation
Based on the empirical footprinting evidence:
● Harden Security Headers:
* Implement Strict-Transport-Security (HSTS) to enforce HTTPS connections.
* Add X-Frame-Options: SAMEORIGIN or DENY and X-Content-Type-Options:
nosniff to prevent clickjacking and MIME-sniffing attacks.
* Configure a strict Content Security Policy (CSP) to control external script execution.
● Banner Grabbing & Server Version Obfuscation:
* Hide detailed BIND DNS server versions (9.16.23-RH) reported by DNS servers.
* Suppress WordPress meta generator headers disclosing exact version numbers
(WordPress 7.1.4, WordPress Download Manager 3.3.76) to lower target profiling
accuracy by automated scanners.
● Mail Security & DMARC:
* Update the SPF record (~all softfail) to a hardfail (-all) once validation is complete and
implement explicit DMARC policies to prevent domain spoofing.
6. Report Sign-Off
* Assessor: Penetration Testing Team
* Target: networkwalks.com
* Status: Footprinting & Passive Reconnaissance Complete
* Evidence Vault Directory: C:\Users\Administrator\Documents\My footprint Result\
