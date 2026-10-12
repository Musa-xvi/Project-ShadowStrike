# Phase 1 — Infrastructure Setup

We're going to build the foundation of Project ShadowStrike using Microsoft Hyper-V, starting with an isolated virtual network and a Windows Server domain controller.

By the end of this setup, you should have a Windows Server virtual machine named `DC01`, running Active Directory Domain Services (AD DS) and DNS, ready for Windows clients to join the domain.

## Objective
- Deploy a Windows Server domain controller, configure Active Directory Domain Services (AD DS), establish DNS, create organizational units and user accounts, and join Windows client machines to the domain.

## Expected Deliverables
- `DC01` VM running Windows Server with AD DS and DNS installed and configured
- An isolated Hyper-V virtual switch/network for the lab
- Documented domain name, domain admin account, initial OU/user lists, and join instructions for client VMs
- Optional: snapshot or checkpoint of `DC01` once fully configured for easy reset

## High-level Steps
1. Prepare Hyper-V host and create an isolated virtual switch for the lab network.
2. Create a new VM named `DC01` with recommended resources (e.g., 2 vCPU, 8 GB RAM, 60 GB disk) and attach an ISO for Windows Server.
3. Install Windows Server on `DC01`, configure networking, and set a static IP.
4. Install Active Directory Domain Services and promote the server to a domain controller for a new domain (choose a lab-safe domain name such as `shadowstrike.lab`).
5. Configure DNS during the AD DS promotion and verify name resolution for the domain.
6. Create Organizational Units (OUs), service accounts, and a set of user accounts for lab use.
7. Create snapshots/checkpoints of `DC01` after configuration.
8. Build or provision Windows client VMs and join them to `shadowstrike.lab`.

## Notes and Tips
- Use a lab-only domain (e.g., `shadowstrike.lab`) to avoid any risk to production DNS.
- If using Hyper-V on Windows 10/11 or Windows Server, prefer generation 2 VMs with Secure Boot disabled only if you need legacy boot.
- Document credentials and store them securely (e.g., password vault). For initial lab testing, use complex but shared passwords and rotate after tests.
- Consider automating repetitive steps (VM creation, OS install unattended, AD DS promotion) with PowerShell/SCVMM/Desired State Configuration.

---

(Previous Phase 1 details retained in repository history; this file focuses on the Hyper-V-oriented infrastructure setup and the `DC01` goal.)
