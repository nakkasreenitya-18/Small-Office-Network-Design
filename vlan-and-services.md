# VLAN and Network Services Design

## 1. VLAN plan

A VLAN (Virtual Local Area Network) creates a separate logical Layer-2 network.

| VLAN ID | Name | Intended devices | Subnet |
|---:|---|---|---|
| 10 | STAFF | Employee laptops, desktops, staff phones | `192.168.10.0/26` |
| 20 | GUEST | Visitor and personal guest Wi-Fi devices | `192.168.10.64/26` |
| 30 | SERVERS | Internal DNS, file/print, and application servers | `192.168.10.128/27` |
| 40 | MANAGEMENT | Switch, access point, and infrastructure management | `192.168.10.160/28` |

## 2. VLAN purpose and controls

### VLAN 10 — Staff

- Used by company-managed employee devices.
- Can access approved internal applications and shared services.
- Internet access is allowed subject to the office security policy.
- Access to management interfaces is restricted to authorised IT administrators.

### VLAN 20 — Guest

- Used by visitors and personal devices.
- Internet access only.
- Must not access staff, server, or management subnets.
- Apply client isolation on the guest wireless network if supported.

### VLAN 30 — Servers

- Hosts approved internal services.
- Accepts connections only on required ports from authorised VLANs.
- Server administration is limited to IT administrators.
- Services should be patched, backed up, and monitored.

### VLAN 40 — Management

- Used only for network-device management.
- Not available to ordinary staff or guest devices.
- Restrict access to designated IT administrator devices.
- Use secure protocols such as SSH and HTTPS; disable insecure management protocols.

## 3. Required network services

| Service | Protocol/port | Purpose | Proposed placement |
|---|---|---|---|
| DHCP | UDP 67/68 | Automatically assigns IP settings | Router/firewall or server |
| DNS | TCP/UDP 53 | Resolves hostnames to IP addresses | Internal DNS server or trusted resolver |
| HTTP | TCP 80 | Unencrypted web traffic; redirect to HTTPS where applicable | Web service |
| HTTPS | TCP 443 | Encrypted web/application access | Internal or external web service |
| SSH | TCP 22 | Secure command-line administration | Network devices/servers; admin sources only |
| SMTP submission | TCP 587 | Authenticated email submission | Approved mail provider/service |
| NTP | UDP 123 | Time synchronisation | Router or approved time source |

Only enable services that the office actually needs. Do not open every listed port automatically.

## 4. Service behaviour

### DHCP

A DHCP server leases addresses to clients. A client normally obtains an IP address, subnet mask, default gateway, and DNS server automatically. Use separate scopes for each client VLAN.

### DNS

DNS translates names such as `intranet.office.example` into IP addresses. Internal records should be available only to authorised internal clients when they contain private information.

### HTTPS

HTTPS protects web traffic using TLS. Internal applications should use valid certificates where practical. Redirect HTTP to HTTPS where the application supports it.

### SSH

SSH is used for secure administration. Permit it only from an authorised administrator workstation or management jump host. Never allow SSH from the guest VLAN.

### NTP

NTP keeps device clocks consistent. Correct time is important for log correlation, certificate validation, and troubleshooting.

## 5. Wireless design

- Staff SSID: mapped to VLAN 10 and protected with WPA2-AES or WPA3.
- Guest SSID: mapped to VLAN 20, with internet-only access.
- Use a strong, unique Wi-Fi passphrase or enterprise authentication where supported.
- Disable WPS if it is not required.
- Keep access-point administration on VLAN 40.

## 6. Inter-VLAN routing

Traffic between VLANs must pass through a Layer-3 switch or router/firewall. Apply access-control lists or firewall policies at this routing point. The guest VLAN should have no route permitted to internal private subnets, except any narrowly required infrastructure such as DHCP/DNS relay handled by the gateway.
