# Phase 2: Configure SIEM, Endpoint Monitoring, and Detection Rules

Objective
- Deploy centralized security telemetry and implement detection rules and endpoint monitoring so the Blue Team can detect, investigate, and respond to stealth-focused tradecraft used by the Red Team.

Key Deliverables
- Centralized log collection (Sysmon/endpoint logs, Windows Event Forwarding, DNS, DHCP, AD logs) ingested into a SIEM or log platform
- Host-based telemetry (EDR) deployed to endpoints and servers with tuned policies for visibility
- Baseline detection rules for common AD abuse (lateral movement, credential theft, persistence, unusual replication or DC access)
- Threat hunting playbooks and alerting workflows integrated with incident response runbooks
- Dashboards and data retention policies to support investigations and forensics

Example Tasks
- Choose and provision SIEM/log store (e.g., Splunk, ELK, Azure Sentinel, Defender for Cloud) and sizing for lab scale
- Configure log forwarders: Windows Event Forwarding/Winlogbeat/Sysmon configuration templates
- Deploy EDR/agent(s) to all endpoints and servers; verify telemetry coverage
- Implement detection rules (e.g., suspicious PowerShell, DCSync, DC shadowing, credential dumping signatures)
- Create correlation rules that combine multiple telemetry sources (DNS + authentication anomalies)
- Build dashboards for visibility (auth anomalies, lateral movement graphs, endpoint anomalies)
- Run validation tests: simulate benign and malicious activity to tune rule thresholds and reduce noise

Notes
- Prioritize high-fidelity signals (EDR, Sysmon) for low-noise detections; capture verbose logs in a separate forensic store where needed.
- Version-control detection rule configs and Sysmon/EDR templates in the repository for repeatable deployments.
