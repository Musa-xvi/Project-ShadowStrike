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
