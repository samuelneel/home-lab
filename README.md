# Active Directory & SOC Detection Home Lab

> 🚧 **Status:** In progress — Phase 1 (Active Directory & Help Desk) underway.

A virtualized lab simulating a small corporate network, built to practice the core skills behind
IT support and security operations: Active Directory administration, endpoint management, help
desk ticketing, log collection, and threat detection.

## Documentation

- [01 — Network Setup](docs/01-network-setup.md)
- [02 — Domain Controller](docs/02-domain-controller.md)
- [03 — Windows 11 Client](docs/03-windows11-client.md)
- [04 — Spiceworks Help Desk](docs/04-spiceworks-helpdesk.md)

## Architecture

```
                          Internet
                             ▲
                             │  (NAT)
        ┌────────────────────┴─────────────────────┐
        │      VirtualBox NAT Network: SamLabNet    │
        │               10.10.10.0/24               │
        │                                           │
        │   ┌──────────────┐      ┌──────────────┐  │
        │   │     DC01     │◄────►│   CLIENT01   │  │
        │   │ Server 2022  │      │  Windows 11  │  │
        │   │ 10.10.10.10  │      │ (domain-     │  │
        │   │ AD DS + DNS  │      │  joined)     │  │
        │   │ samlab.local │      │              │  │
        │   └──────────────┘      └──────────────┘  │
        └───────────────────────────────────────────┘

   Spiceworks Cloud Help Desk (SaaS) ── ticket workflow ──► fix performed in AD on DC01
```

## Environment

| Component         | Value                                               |
|-------------------|-----------------------------------------------------|
| Hypervisor        | Oracle VirtualBox 7.x                               |
| Domain Controller | Windows Server 2022 (Desktop Experience), `DC01`    |
| Client            | Windows 11, `CLIENT01`                              |
| Domain            | `samlab.local` (NetBIOS: `SAMLAB`)                  |
| Network           | VirtualBox NAT Network `SamLabNet`, `10.10.10.0/24` |
| Ticketing         | Spiceworks Cloud Help Desk                          |

| Host     | IP            | Mask            | Gateway      | DNS           |
|----------|---------------|-----------------|--------------|---------------|
| DC01     | `10.10.10.10` | `255.255.255.0` | `10.10.10.1` | `127.0.0.1`   |
| CLIENT01 | DHCP          | DHCP            | DHCP         | `10.10.10.10` |

## Tools & Technologies

- **Hypervisor:** VirtualBox (free)
- **Domain Controller:** Windows Server 2022 (Evaluation edition, free 180 days)
- **Client:** Windows 11
- **Ticketing:** Spiceworks Cloud Help Desk (free, cloud-hosted)
- **SIEM (Phase 2):** Splunk Free or Wazuh
- **Attack simulation (Phase 2):** manual failed-logon attempts / Atomic Red Team

## Phase 1 — Active Directory & Help Desk

- [x] Created a VirtualBox NAT Network (`SamLabNet`, `10.10.10.0/24`) for VM-to-VM and internet connectivity
- [x] Installed Windows Server 2022 and promoted it to a domain controller (`DC01`), creating the forest `samlab.local`
- [x] Built an OU structure and provisioned test users and security groups in Active Directory
- [x] Joined a Windows 11 client (`CLIENT01`) to the domain
- [ ] Configured a Group Policy (e.g., password complexity, login banner)
- [ ] Stood up Spiceworks Cloud Help Desk and resolved a simulated account-lockout ticket end to end (open → assign → fix in AD → close)

## Phase 2 — SOC Detection (planned)

- [ ] Deploy the SIEM VM and forward Windows event logs from the client and DC
- [ ] Generate activity, including repeated failed logons to simulate a brute-force attempt
- [ ] Search the logs and build an alert/detection for the failed-logon pattern (Windows Event ID 4625)
- [ ] Document the investigation: what the alert caught, how it was confirmed, and what a defender would do next

## Skills Demonstrated

- Active Directory Domain Services: forest/domain deployment, OU design, user and group provisioning
- DNS fundamentals: AD-integrated zone, name-resolution troubleshooting (`nslookup`)
- Windows client administration: domain join, TPM/firmware configuration, OOBE
- Networking: VirtualBox NAT Network, static IP assignment, DNS configuration (TCP/IP)
- Group Policy configuration (password policy, login banner)
- Help desk operations: account creation, password resets, lockouts; full ticket lifecycle in Spiceworks
- *(Phase 2)* Log collection and analysis with a SIEM; detecting and investigating a simulated attack

## Future Improvements

- Add a firewall (pfSense) and segment the network
- Add a Linux endpoint and forward its logs
- Automate user creation with a PowerShell script
- Extend identity into the cloud with Microsoft Entra ID (synced to on-prem AD via Entra Connect)
