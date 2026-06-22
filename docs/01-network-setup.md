# 01 — Network Setup

## Objective

Create an isolated virtual network in VirtualBox that lets the domain controller and client
communicate with each other and reach the internet which is the foundation the `samlab.local` domain
is built on. 

## Addressing plan

| Host     | IP            | Mask            | Gateway      | DNS           |
|----------|---------------|-----------------|--------------|---------------|
| DC01     | `10.10.10.10` | `255.255.255.0` | `10.10.10.1` | `127.0.0.1`   |
| CLIENT01 | DHCP          | DHCP            | DHCP         | `10.10.10.10` |

The domain controller uses a **static IP** and points DNS at **itself** (it runs the DNS role).
The client pulls its IP from the NAT Network's DHCP but is manually pointed at the DC for DNS, so
it can resolve `samlab.local` and locate the domain.

## Steps

1. The Network Manager is hidden in VirtualBox's **Basic Mode**, so I switched the VirtualBox
   Manager to **Expert Mode**.
2. Opened **File → Tools → Network** to launch the Network Manager.
3. On the **NAT Networks** tab, clicked **Create**, then opened **Properties** to configure it:
   - **Name:** `SamLabNet`
   - **IPv4 Prefix:** `10.10.10.0/24`
   - **DHCP:** enabled
   - **IPv6:** disabled
4. Clicked **Apply**. Each VM is then attached to this network via
   **Settings → Network → Adapter 1 → Attached to: NAT Network → Name: `SamLabNet`**.

![NAT Network SamLabNet created in the VirtualBox Network Manager](../screenshots/01-network/nat-network-created.png)

## Skills demonstrated

- VirtualBox network configuration
- IP addressing and subnetting (`10.10.10.0/24`)
- Planning DNS for an Active Directory environment
