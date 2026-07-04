# Active Directory & ML Log Analysis Home Lab

> 🚧 **Status:** Phase 1 complete — Phase 2 (ML-based log analysis) planned.

A virtualized lab simulating a small corporate network, built to practice the core skills behind
IT support and data-driven operations: Active Directory administration, endpoint management, help
desk ticketing, and log analysis with Python — using the lab's own Windows event logs as a real
dataset for anomaly detection.

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
- **Log analysis (Phase 2):** Python, pandas, matplotlib, scikit-learn, Jupyter
- **Data generation (Phase 2):** PowerShell (`Get-WinEvent`), manual failed-logon attempts

## Phase 1 — Active Directory & Help Desk

- [x] Created a VirtualBox NAT Network (`SamLabNet`, `10.10.10.0/24`) for VM-to-VM and internet connectivity
- [x] Installed Windows Server 2022 and promoted it to a domain controller (`DC01`), creating the forest `samlab.local`
- [x] Built an OU structure and provisioned test users and security groups in Active Directory
- [x] Joined a Windows 11 client (`CLIENT01`) to the domain
- [x] Configured a Group Policy (e.g., password complexity, login banner)
- [x] Stood up Spiceworks Cloud Help Desk and resolved a simulated account-lockout ticket end to end (open → assign → fix in AD → close)

## Phase 2 — ML Log Analysis: Anomaly Detection on Logon Events (planned)

- [ ] Generate activity on the domain, including repeated failed logons to simulate a brute-force attempt
- [ ] Export Windows Security event logs from DC01 and CLIENT01 to CSV using PowerShell (`Get-WinEvent`)
- [ ] Parse and clean the logs in Python with pandas (event ID, account, source host, timestamp)
- [ ] Engineer features from the raw events (e.g., failed logons per account per time window) and establish a baseline of normal logon behavior
- [ ] Detect the brute-force pattern (Event ID 4625) two ways — a statistical threshold baseline, then a simple ML model (Isolation Forest) — and compare their results
- [ ] Visualize logon activity and flagged anomalies over time with matplotlib
- [ ] Document the full analysis in a Jupyter notebook: what was flagged, false positives, and how detection quality was evaluated

## Skills Demonstrated

- Active Directory Domain Services: forest/domain deployment, OU design, user and group provisioning
- DNS fundamentals: AD-integrated zone, name-resolution troubleshooting (`nslookup`)
- Windows client administration: domain join, TPM/firmware configuration, OOBE
- Networking: VirtualBox NAT Network, static IP assignment, DNS configuration (TCP/IP)
- Group Policy configuration (password policy, login banner)
- Help desk operations: account creation, password resets, lockouts; full ticket lifecycle in Spiceworks
- *(Phase 2)* Data analysis with Python and pandas; feature engineering and anomaly detection on real Windows event logs; data visualization and notebook-based documentation

## Future Improvements

- Automate user creation with a PowerShell script
- Schedule recurring activity generation to grow the log dataset over time
- Try additional models on the logon data and compare against the Isolation Forest baseline
- Extend identity into the cloud with Microsoft Entra ID (synced to on-prem AD via Entra Connect)
