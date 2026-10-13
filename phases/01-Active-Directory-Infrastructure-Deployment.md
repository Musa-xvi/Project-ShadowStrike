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

## 2. Create the isolated Hyper-V virtual switch

On your physical Windows computer:
1. Open Hyper-V Manager.
2. Select your computer in the left panel.
3. Click Virtual Switch Manager.
4. Select "Private" and click Create Virtual Switch.
5. Enter the name ShadowStrike-LAN.
6. Click Apply, then OK.

<img width="1712" height="851" alt="image" src="https://github.com/user-attachments/assets/1f3d07e5-a0c4-48c7-8fff-f63096c993a4" />

## 3. Create the DC01 virtual machine

First, obtain a legitimate Windows Server 2022 or 2025 ISO from the Microsoft Evaluation Center.

In Hyper-V Manager, click New → Virtual Machine and configure the following:

Setting | Recommended value
--- | ---
VM name | DC01
Generation | Generation 2
Startup memory | 4096 MB
Dynamic memory | Disabled initially
Processors | 2 virtual CPUs
Virtual hard disk | 80 GB VHDX
Network connection | ShadowStrike-LAN
Installation media | Windows Server ISO

Finish the wizard, then open `DC01` → Settings → Processor to assign 2 virtual processors (if not set during creation).

Start the VM and install Windows Server. For easier administration during the lab, choose the Desktop Experience installation option.

After installation, sign in with the local `Administrator` account.

<img width="1073" height="801" alt="image" src="https://github.com/user-attachments/assets/203a0d38-81cb-4e56-8726-f6b9aa3b1c29" />

<img width="1917" height="820" alt="image" src="https://github.com/user-attachments/assets/793d6de5-26e1-44d0-bfce-72a6cbdccaa9" />

## 4. Configure DC01's static IP address

Inside DC01, open PowerShell as Administrator.
First, identify the network adapter:

<img width="893" height="128" alt="image" src="https://github.com/user-attachments/assets/0d6419c6-d485-49f9-b5f8-9a012c2f8e8f" />

Assign a static IPv4 address to the network adapter named Ethernet:

<img width="546" height="499" alt="image" src="https://github.com/user-attachments/assets/a8818562-5b46-43ed-bfbc-23cad117320d" />

We intentionally omit the default gateway because this network is isolated.

Configure DNS:

<img width="446" height="59" alt="image" src="https://github.com/user-attachments/assets/33d76dde-3aea-4c54-be11-2a3a58eb1d83" />

Rename the server:

Rename-Computer -NewName "DC01" -Restart

After restarting, verify the configuration:

<img width="650" height="467" alt="image" src="https://github.com/user-attachments/assets/6ab89a85-ba7f-4d55-b1b3-90eb16127c8b" />

You should see the IP address 10.10.10.10 and hostname DC01.

## 5. Install Active Directory Domain Services

On DC01, open an elevated PowerShell window:

<img width="731" height="123" alt="image" src="https://github.com/user-attachments/assets/f48f5b69-b8d5-4824-a123-251991c915eb" />

Verify Installation

<img width="721" height="135" alt="image" src="https://github.com/user-attachments/assets/45596b7c-f7de-4561-8cb1-a6987ffe965c" />

Next, promote the server to a domain controller and create the ShadowStrike forest:

You'll be prompted to enter and confirm a Directory Services Restore Mode (DSRM) password. Store it securely.

<img width="795" height="184" alt="image" src="https://github.com/user-attachments/assets/62ef441b-3748-4ac7-a22e-d9df6d6fb94a" />

## 6. Validate the domain controller

After the restart, sign in using the domain Administrator account using admin credentials

# Confirm domain details

Get-ADDomain

<img width="944" height="549" alt="image" src="https://github.com/user-attachments/assets/7072ea4d-669b-4361-a4c5-c9733cc11b5e" />

# Confirm the AD forest

<img width="986" height="328" alt="image" src="https://github.com/user-attachments/assets/b453a7c7-83f7-4bab-a29e-07627b82765f" />

# Verify DNS resolution

<img width="753" height="116" alt="image" src="https://github.com/user-attachments/assets/a79bcf19-5ccc-492a-b921-f8c94780982b" />

# Check domain controller health

 dcdiag
 
<img width="913" height="281" alt="image" src="https://github.com/user-attachments/assets/771cc843-257d-4d90-9b26-d0737493bd9c" />

# Confirm DNS and AD services

<img width="621" height="121" alt="image" src="https://github.com/user-attachments/assets/2e4a131e-c3c6-4b8c-8105-15e182b98dcc" />

# Check SYSVOL and NETLOGON shares

<img width="637" height="216" alt="image" src="https://github.com/user-attachments/assets/44fee790-4ae6-477f-b751-3565cc53d4b6" />

## 7. Create and join CLIENT01

Create a Windows client VM to join the domain and validate client connectivity.

In Hyper-V Manager, create a new VM and configure it as follows:

Setting | Value
--- | ---
VM name | CLIENT01
Operating system | Windows 11 Pro or Enterprise
Memory | 4 GB
CPU | 2 vCPUs
Storage | 64 GB
Virtual switch | ShadowStrike-LAN

After creating the VM, attach a Windows 11 ISO, start CLIENT01, and install the OS. Once installed, sign in to the local Administrator account and configure the network adapter:

- Set a static IP in the 10.10.10.0/24 range (for example, 10.10.10.20)
- Subnet mask: 255.255.255.0
- Preferred DNS server: 10.10.10.10 (DC01)

Rename the computer to CLIENT01 and restart.

Join CLIENT01 to the domain `ad.shadowstrike.test`:

1. Open System Properties → Change settings → Change
2. Select Domain and enter `ad.shadowstrike.test`
3. When prompted, authenticate with the domain Administrator account: SHADOWSTRIKE\Administrator
4. After successful join, restart CLIENT01

Validation:
- From CLIENT01, run `ping DC01` and `nslookup ad.shadowstrike.test` to confirm name resolution and connectivity.
- Confirm you can log on to CLIENT01 with domain credentials (e.g., SHADOWSTRIKE\Administrator or a created test user).

Notes:
- If you have issues joining, verify DNS settings on CLIENT01 point to DC01 and that DC01's DNS has the _msdcs and domain records.
- If the host firewall is blocking, temporarily allow necessary traffic (RPC, Kerberos, LDAP) for the join operation.


