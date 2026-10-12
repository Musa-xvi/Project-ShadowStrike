# Phase 4: Monitoring, Logging & Forensics

Objective
- Enhance and validate logging, collection, and forensic readiness so investigations can be performed efficiently and reliably after detection or incidents.

Key Deliverables
- Centralized forensic store for verbose logs and raw artifacts
- Network capture points (taps/span) or packet capture solutions on critical segments
- Forensic acquisition playbooks for endpoints and servers
- SIEM enrichment and retention policies aligned with forensic needs
- Routine validation of log integrity and completeness

Example Tasks
- Deploy PCAP collection for core network zones and schedule rolling captures where feasible
- Create forensic capture playbooks (disk images, Volatility profiles, timeline creation)
- Validate that all critical logs (AD DS, DNS, DHCP, EDR telemetry) are forwarded and queryable
- Implement immutable storage or write-once retention for critical forensic artifacts
