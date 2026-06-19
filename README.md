# Active Directory & SOC Detection Home Lab

> 🚧 **Status:** In progress — Phase 1 (Active Directory) underway.

A virtualized lab simulating a small corporate network, built to practice the
core skills behind IT support and security operations: Active Directory
administration, endpoint management, log collection, and threat detection.

## Skills Demonstrated
- Windows Server / Active Directory Domain Services administration
- User, group, and Organizational Unit (OU) management
- Group Policy configuration
- Common help desk tasks: account creation, password resets, account lockouts
- Log collection and analysis with a SIEM
- Detecting and investigating a simulated attack

## Architecture
[Insert a simple diagram here — draw.io is free. Show: Domain Controller (Windows
Server), one or more Windows client VMs joined to the domain, and the SIEM VM,
all on the same virtual network.]

## Tools & Technologies
- **Hypervisor:** VirtualBox (free)
- **Domain Controller:** Windows Server 2022 (Evaluation edition, free 180 days)
- **Client:** Windows 10/11
- **SIEM:** Splunk Free (or Wazuh)
- **Attack simulation:** manual failed-logon attempts / Atomic Red Team

## Phase 1 — Active Directory
1. Installed VirtualBox and created the VMs on an internal virtual network.
2. Installed Windows Server, promoted it to a Domain Controller, and created the
   domain `samlab.local`.
3. Created OUs (e.g., IT, Sales), users, and security groups.
4. Joined a Windows 10 client to the domain.
5. Configured a Group Policy (e.g., password complexity, login banner).
6. Practiced help desk workflows: created a new-hire account, reset a password,
   and unlocked a locked-out account.

[Insert screenshots: domain creation, the OU structure, a domain-joined client,
a password reset.]

## Phase 2 — SOC Detection
1. Deployed the SIEM VM and forwarded Windows event logs from the client and DC.
2. Generated activity, including repeated failed logons to simulate a brute-force
   attempt.
3. Searched the logs and built an alert/detection for the failed-logon pattern
   (Windows Event ID 4625).
4. Documented the investigation: what the alert caught, how I confirmed it, and
   what a defender would do next.

[Insert screenshots: log forwarding working, the failed-logon events, the alert.]

## Key Takeaways
[2–4 sentences in your own words on what you learned — what was harder than
expected, what clicked. This section is what interviewers actually read.]

## Future Improvements
- Add a firewall (pfSense) and segment the network
- Add a Linux endpoint and forward its logs
- Automate user creation with a PowerShell script
