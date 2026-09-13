# Open Source Security Operations Center (SOC)

Design and deployment of a fully functional **Security Operations Center (SOC)** built entirely on open source tools, in a segmented and virtualized network environment. Final-year project (PFE) — TEK-UP University, Network Security & Information Systems (ING4-J SSIR-H).

## Overview

The project implements the complete operational SOC cycle:

**Monitor → Detect → Analyse → Respond**

using **pfSense** (firewall/router + Suricata IDS/IPS), **Wazuh** (SIEM/EDR), **TheHive** (incident case management), and **Shuffle** (SOAR automation). Real attack scenarios were simulated from a **Kali Linux** machine (Nmap scan, SSH brute-force) to validate detection and automated response across the full chain.

**Keywords:** SOC, SIEM, SOAR, pfSense, Wazuh, Suricata, TheHive, Shuffle, Cybersecurity, Open Source.

## Architecture

- Segmented **WAN / LAN / DMZ** network built on **pfSense**, with strict firewall filtering rules between zones.
- **Suricata** IDS/IPS integrated directly into pfSense for real-time network-based detection.
- **Wazuh** SIEM/EDR centralizing logs from all endpoints (Linux, Windows, DMZ agents), with File Integrity Monitoring (FIM) and multi-source correlation.
- **TheHive** for structured incident case management, fed by alerts forwarded through **Shuffle**.
- **Shuffle** SOAR workflows connecting Wazuh alerts to TheHive cases (no-code automation via the Wazuh `ossec.conf` integration hook).
- **DVWA** deployed in the DMZ as an intentionally vulnerable target application.

## Tools Used

| Tool | Role |
|---|---|
| pfSense | Firewall, router, network segmentation |
| Suricata | Network IDS/IPS (integrated on pfSense) |
| Wazuh | SIEM / EDR, log collection & correlation |
| TheHive | Incident / case management |
| Shuffle | SOAR — workflow automation |
| DVWA | Vulnerable web app (attack target, DMZ) |
| Kali Linux | Attack simulation (Nmap, Hydra) |

## Attack Scenarios & Validation

1. **Nmap reconnaissance scan** against the DVWA server in the DMZ (`nmap -sV -sC -p- 192.168.20.20`) and a subnet sweep (`nmap -sn 192.168.20.0/24`).
2. **SSH brute-force** against a Windows host using Hydra (`hydra -l admin -P rockyou.txt ssh://192.168.20.10`).

Both scenarios were successfully detected end-to-end: alerts flowed from **Suricata/Wazuh → Shuffle → TheHive**, confirming full integration of the Monitor–Detect–Analyse–Respond cycle.

## Key Results

- WAN/LAN/DMZ network segmentation on pfSense with strict filtering rules.
- Real-time network detection via Suricata IDS/IPS.
- Centralized log collection and correlation across endpoints via Wazuh.
- Automated SOAR workflows (Shuffle) linking Wazuh to TheHive.
- Full detection chain validated against real attack simulations.

## Performance & Limitations (qualitative)

| Component | Strengths | Limitations | Improvement Ideas |
|---|---|---|---|
| pfSense / Suricata | Real-time network detection, ET rules | High CPU load in IPS mode | Enable multi-threading |
| Wazuh | Multi-source correlation, built-in FIM | Latency on large volumes | Tune correlation rules |
| Shuffle | Fast no-code automation | External API dependency | Add Threat Intelligence feed |
| TheHive | Structured case management | Interface sometimes slow | Upgrade to TheHive 5.x |

## Future Improvements

- Integrate a Threat Intelligence platform (**MISP**) to enrich alerts with IOCs.
- Add ML-based behavioral detection rules in Wazuh.
- Deploy a honeypot for early reconnaissance detection.
- Extend monitoring coverage to web applications (WAF).
- Implement automated active response (IP blocking via the pfSense API, triggered from Shuffle).

## Report Structure

The full report (French, 33 pages) includes:
- Résumé / Abstract, table of contents, list of figures, list of tables
- General introduction (context, problem statement, objectives)
- State of the art on SOC concepts and the tools used, with a comparison to proprietary solutions
- Solution architecture (network diagram, data flow, IP addressing, hardware/software)
- Deployment & configuration (pfSense, Suricata, Wazuh, DVWA, Shuffle)
- Tests, results and validation (attack scenarios, dashboards, incident handling in TheHive, performance analysis)
- General conclusion and perspectives, bibliography, and an appendix with the commands used

## Authors

- **Hamdi Maïssa**
- **Reguigui Aymen**
- Supervised by Sahar Ben Yaala — TEK-UP University, 2025–2026
