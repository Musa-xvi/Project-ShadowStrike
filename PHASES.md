# Project Phases

## Phase 1: Active Directory Infrastructure Deployment

Objective
- Build a realistic, enterprise-grade Active Directory (AD) lab environment that mirrors common organizational structure and services so Red and Blue teams can operate against a representative target.

Key Deliverables
- Domain Controller(s) deployed and configured (AD DS, DNS, DHCP as required)
- Member servers (file servers, application servers) and workstations joined to the domain
- Organizational Units (OUs), user and group structure seeded for realistic permissions
- Service accounts, Group Policy Objects (GPOs), and sample services (e.g., AD CS, IIS) where appropriate
- Network segmentation and basic firewall rules to emulate enterprise network zones
- Baseline snapshots/backups for easy reset between exercises

Example Tasks
- Design AD topology and naming conventions
- Provision VMs or cloud instances for DCs, servers, and endpoints
- Install and promote Domain Controllers
- Configure DNS, DHCP, and time services
- Create OUs, users, groups, service accounts, and initial GPOs
- Harden default configurations to a defined baseline (document deviations for Red Teaming)
- Create scripted automation (IaC) to deploy/teardown the environment

Notes
- Keep the environment modular and easily resettable — use snapshots or IaC tooling (Terraform/Ansible) where possible.
- Document all credentials, service account privileges, and network topology for both teams.

---

## Phase 2: Configure SIEM, Endpoint Monitoring, and Detection Rules

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

---

## Phase 3: Conduct Controlled Red Team Attack Simulations

Objective
- Execute planned, controlled attack simulations targeting the Phase 1 AD environment to validate detections, test Blue Team responses, and exercise realistic stealthy techniques.

Key Deliverables
- Red Team playbooks/scenarios documenting goals, allowed techniques, and rollback procedures
- Attack runs scheduled and coordinated with Blue Team detection/response windows
- Evidence collection (logs, packet captures, screenshots) and a scoring/feedback mechanism to measure detection and response effectiveness
- Post-exercise reports detailing techniques used, detection coverage, gaps, and recommended mitigations

Example Tasks
- Define rules of engagement, scope, and safe-guards for each simulation
- Execute phased intrusion scenarios: initial access, privilege escalation, lateral movement, persistence, data exfiltration attempts (simulated)
- Use stealth-focused tooling and tradecraft with emphasis on low-and-slow techniques
- Coordinate live exercises with Blue Team to test real-time detection and response, and also run blind/emulated tests
- Capture and tag telemetry artifacts for later analysis and forensics
- Produce after-action reviews and update detection rules and playbooks based on findings

Notes
- Ensure careful coordination and backups before each simulation; provide a communication channel for emergency stops.
- Treat some exercises as training (guided) and others as evaluation (unguided) to get both coaching and measurement benefits.

---

## Phase 4: Monitoring, Logging & Forensics
- Placeholder: deploy centralized logging, SIEM rules, and forensic data collection.

## Phase 5: Hardening, Remediation & Reporting
- Placeholder: apply mitigations, retest, and produce final reports and playbooks.
