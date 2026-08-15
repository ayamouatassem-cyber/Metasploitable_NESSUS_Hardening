# Metasploitable_NESSUS_Hardening


## Executive Summary
This project demonstrates a full vulnerability management lifecycle conducted in an isolated lab environment. Using Tenable Nessus Essentials, an initial baseline scan was executed against an unhardened Metasploitable 2 target (192.168.56.101), discovering 68 total vulnerabilities.

Through targeted system hardening, service reconfigurations, and attack surface reduction, total vulnerabilities were reduced to 50—a net elimination of 18 vulnerabilities, including full removal of open NFS share exposures and Samba Badlock risks.

---

## Scan Results Comparison

| Scan Phase | Total Vulnerabilities | High / Critical Focus Areas |
| :--- | :---: | :--- |
| Initial Scan Baseline | 68 | NFS World Readable, Samba Badlock, BIND DoS, Weak SSL Ciphers, Ghostcat, HTTP TRACE/DELETE |
| Post-Remediation Scan | 50 | -18 Vulnerabilities (NFS and Samba Highs eliminated, Apache SSL/HTTP hardened) |

### Dashboards Evidence
* Initial Scan Baseline (68 Vulnerabilities):
  ![Initial Scan](screenshots/.png)

* Post-Remediation Scan (50 Vulnerabilities):
  ![Post-Remediation Scan](screenshots/post-remediation-50-vulns.png)

---

## Key Technical Remediations and Risk Management

### 1. Network File System (NFS) Share Restrictions (Port 2049)
* Finding: /etc/exports exported root directories with wildcard (*) read/write access to any remote host.
* Remediation: Modified /etc/exports to restrict mount permissions to specific lab subnets and enforced root_squash to prevent privilege escalation.

### 2. Web Server Hardening and Cipher Optimization (Apache2 / Port 443)
* Finding: Apache enabled legacy SSL protocols (SSLv2/SSLv3) and weak 64-bit/128-bit ciphers (SWEET32 / 3DES / RC4).
* Remediation: Enforced SSLCipherSuite HIGH and SSLProtocol -ALL +TLSv1 directives inside Apache ports.conf and site configuration modules.

### 3. Service Surface Reduction (Samba and BIND)
* Finding: Legacy Samba (3.0.20) and ISC BIND (9.4.2) services contained unpatched Remote Code Execution (RCE) flaws on an unpatchable legacy OS (Ubuntu 8.04).
* Mitigation: Executed service deprecation by stopping smbd, nmbd, and bind9 daemons and removing them from startup runlevels via update-rc.d.

### 4. Blocking Risky HTTP Methods
* Finding: Apache web server enabled dangerous HTTP methods (TRACE, TRACK, DELETE, PUT).
* Remediation: Enabled mod_rewrite and injected directory rules returning 403 Forbidden for non-essential HTTP verbs, alongside setting TraceEnable off.

### 5. Apache Tomcat AJP Connector Analysis (Ghostcat / Port 8009)
* Finding: Apache Tomcat 5.5 exposed an unauthenticated AJP connector vulnerable to arbitrary file reading and remote code execution (CVE-2020-1938).
* Risk Status: Identified as a Critical legacy finding. Documented and deferred to Phase 2 remediation due to core application dependencies, with recommended action to comment out the secretRequired AJP connector in server.xml or stop the tomcat daemon.

---

## Technical Challenges and Engineering Lessons

* Apache Configuration Precedence: Overcame issues where global directives in apache2.conf were overridden by legacy sites-available/default-ssl and ports.conf files.
* Modern vs. Legacy OpenSSL Policies: Solved verification mismatches caused by Kali Linux OpenSSL 3.x security levels (SECLEVEL=2) blocking initial legacy handshakes during manual terminal testing.
* Memory Process Cleanup: Identified lingering orphaned web processes retaining old configurations across service reloads, requiring explicit process termination (killall -9 apache2) prior to restarting.

---

## Repository Structure

```text
├── README.md                      <-- Main project documentation
├── docs/
│   ├── executive-summary.pdf      <-- Formal executive stakeholder report
│   └── remediation-matrix.csv     <-- Granular technical remediation log
└── screenshots/
    ├── initial-scan-68-vulns.png  <-- Initial scan baseline
    └── post-remediation-50-vulns.png <-- Final post-remediation scan
