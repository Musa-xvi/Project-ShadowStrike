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

## 1. Network architecture
We'll keep the existing Wi-Fi network separate from the Active Directory lab.

Setting | Configuration
--- | ---
Hypervisor | Microsoft Hyper-V
Physical host IP | 192.168.8.149 (DHCP)
Home router | 192.168.8.1
Virtual switch | ShadowStrike-LAN
Virtual switch type | Private
Lab subnet | 10.10.10.0/24
Domain controller | DC01
DC01 IP address | 10.10.10.10
Subnet mask | 255.255.255.0
Default gateway | None initially
Preferred DNS | 10.10.10.10
AD domain | ad.shadowstrike.test

Why a Private switch?
- A Private virtual switch allows virtual machines attached to the same switch to communicate with each other without directly connecting them to your physical host or home network. For the initial deployment, this provides a useful level of isolation. We'll attach `CLIENT01`, `CLIENT02`, and eventually the Red Team and Blue Team machines to this virtual network.

Important: With this configuration, `DC01` won't have internet access. We'll handle Windows updates and any required downloads separately, without exposing the domain controller to the home LAN.

2. ## Create the isolated Hyper-V virtual switch

On your physical Windows computer:
1. Open Hyper-V Manager.
2. Select your computer in the left panel.
3. Click Virtual Switch Manager.
4. Select Private and click Create Virtual Switch.
5. Enter the name ShadowStrike-LAN.
6. Click Apply, then OK.
 <img width="1712" height="851" alt="image" src="https://github.com/user-attachments/assets/1f3d07e5-a0c4-48c7-8fff-f63096c993a4" />




## Notes and Tips
- Use a lab-only domain (e.g., `shadowstrike.lab`) to avoid any risk to production DNS.
- If using Hyper-V on Windows 10/11 or Windows Server, prefer generation 2 VMs with Secure Boot disabled only if you need legacy boot.
- Document credentials and store them securely (e.g., password vault). For initial lab testing, use complex but shared passwords and rotate after tests.
- Consider automating repetitive steps (VM creation, OS install unattended, AD DS promotion) with PowerShell/SCVMM/Desired State Configuration.

---

(Previous Phase 1 details retained in repository history; this file focuses on the Hyper-V-oriented infrastructure setup and the `DC01` goal.)
