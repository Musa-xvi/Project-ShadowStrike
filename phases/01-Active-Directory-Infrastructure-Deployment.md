# Phase 1: Active Directory Infrastructure Deployment

Objective
- Deploy a Windows Server domain controller, configure Active Directory Domain Services (AD DS), establish DNS, create organizational units and user accounts, and join Windows client machines to the domain.

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
