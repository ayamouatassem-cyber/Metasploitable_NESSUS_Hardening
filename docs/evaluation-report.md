# Post-Hardening Security Evaluation Report

## 1. Executive Summary
This evaluation details the security posture improvements achieved on the Metasploitable 2 target environment (192.168.56.101) following the execution of system hardening protocols, service reconfigurations, and attack surface reduction.

## 2. Quantitative Scan Metrics Comparison
* Pre-Hardening Total Vulnerabilities: 68
* Post-Hardening Total Vulnerabilities: 50
* Risk Reduction Percentage: 26.5% overall finding reduction (including elimination of major High/Critical NFS and Samba exposure vectors)

| Severity Level | Pre-Hardening Findings | Post-Hardening Findings | Delta / Status |
| :--- | :--- | :--- | :--- |
| **Critical** | 2 | 2 | Deferred (Acknowledged Ghostcat and Bind Shell backdoors) |
| **High** | 12 | 4 | Reduced (Eliminated NFS disclosures and Samba Badlock) |
| **Medium** | 18 | 12 | Mitigated (Hardened Apache SSL ciphers and HTTP methods) |
| **Low / Info** | 36 | 32 | Accepted Risk (Legacy system info and operational protocols) |

## 3. Residual Risk and Recommendations
* Accepted Risks: Apache Tomcat AJP connector (Ghostcat / Port 8009) and the unauthenticated inetd root listener (Port 1524) remain open to preserve core legacy lab baseline functionality.
* Future Hardening: Implement host-based firewall filtering (iptables) to restrict access to Port 1524, comment out secretRequired AJP connectors in Tomcat server.xml, and place the target behind network segmentation with HIDS monitoring.
