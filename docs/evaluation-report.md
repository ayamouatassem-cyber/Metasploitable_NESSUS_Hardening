# Post-Hardening Security Evaluation Report

## 1. Executive Summary
This evaluation details the security posture improvements achieved on the Metasploitable 2 target environment (192.168.56.101) following the execution of system hardening protocols, service reconfigurations, and attack surface reduction.

## 2. Quantitative Scan Metrics Comparison
* Pre-Hardening Total Vulnerabilities: 68
* Post-Hardening Total Vulnerabilities: 50
* Risk Reduction Percentage: 26.5% overall finding reduction (including 100% elimination of High severity risks and a 50% drop in Critical severity findings)

| Severity Level | Pre-Hardening Findings | Post-Hardening Findings | Delta / Status |
| :--- | :--- | :--- | :--- |
| **Critical** | 6 | 3 | Reduced (-3 Resolved: Samba and NFS root exposures) |
| **High** | 3 | 0 | Resolved (-3 Resolved: 100% High risks eliminated) |
| **Mixed** | 7 | 5 | Mitigated (-2 Resolved: Hardened Apache SSL/HTTP) |
| **Low** | 3 | 3 | Accepted Risk (Unchanged) |
| **Info** | 49 | 39 | Informational (General system telemetries and services) |

## 3. Residual Risk and Recommendations
* Accepted Risks: The 3 remaining Critical findings—including the Apache Tomcat AJP connector (Ghostcat / Port 8009) and the unauthenticated inetd root listener (Port 1524)—remain open to preserve core legacy lab baseline functionality.
* Future Hardening: Implement host-based firewall filtering (iptables) to restrict access to Port 1524, comment out secretRequired AJP connectors in Tomcat server.xml, and place the target behind network segmentation with HIDS monitoring.
