# Small-Office Network & Service Design

**Project type:** Computer Networks capstone  
**Scenario:** Secure network for a small office with up to 30 employees  
**Prepared for:** Computer Networks course submission

## 1. Project objective

Design a practical, segmented, and manageable network for a small office. The design covers IP addressing and subnetting, VLAN separation, essential network services, basic firewall policy, and a proposed network topology.

The design is a **planning document**, not a claim that physical equipment has been installed or tested. The IP ranges and policies are proposed examples and should be adapted to the actual office if implemented.

## 2. Assumptions

- One office floor with up to 30 employees.
- One internet connection terminating at an edge router/firewall.
- One managed Layer-2/Layer-3 switch (or a router-on-a-stick arrangement).
- One wireless access point supporting separate staff and guest SSIDs.
- A small internal server or virtual machine hosts approved internal services.
- The office uses private IPv4 addressing from `192.168.10.0/24`.
- Guest devices must not reach internal office devices.
- Network equipment management is restricted to IT administrators.

## 3. Proposed design at a glance

| VLAN | Purpose | Subnet | Default gateway |
|---|---|---|---|
| 10 | Staff devices | `192.168.10.0/26` | `192.168.10.1` |
| 20 | Guest Wi-Fi | `192.168.10.64/26` | `192.168.10.65` |
| 30 | Servers and shared services | `192.168.10.128/27` | `192.168.10.129` |
| 40 | Network management | `192.168.10.160/28` | `192.168.10.161` |

The remaining address space is reserved for future growth and additional subnets.

## 4. Main design decisions

1. **Use VLANs** to separate staff, guest, server, and management traffic.
2. **Use DHCP** to assign addresses automatically to staff and guest clients.
3. **Use static or reserved addresses** for servers and network infrastructure.
4. **Use DNS** for name resolution.
5. **Use HTTPS and SSH** for secure application access and administration.
6. **Apply least-privilege firewall rules:** permit only required traffic.
7. **Keep guest Wi-Fi isolated** from internal networks.

## 5. Repository contents

- [`subnet-design.md`](subnet-design.md) — subnet calculations and address plan.
- [`vlan-and-services.md`](vlan-and-services.md) — VLANs, services, and service placement.
- [`firewall-rules.md`](firewall-rules.md) — proposed traffic policy.
- [`topology.md`](topology.md) — network topology diagram (Mermaid).
- [`implementation-and-testing.md`](implementation-and-testing.md) — implementation sequence and validation checklist.

## 6. Important security notes

- Change default passwords on all network equipment.
- Use WPA2-AES or WPA3 for Wi-Fi; disable WPS where possible.
- Use MFA for remote access and administrative accounts where supported.
- Keep router, switch, access point, server, and endpoint software updated.
- Back up network configurations.
- Log administrative access and firewall events.
- Do not expose SSH, RDP, or device-management interfaces directly to the public internet.

## 7. Conclusion

This design provides a simple starting point for a small office. Subnetting limits broadcast domains and organizes address allocation, VLANs separate different device groups, and firewall rules reduce unnecessary access between them. The design can be expanded by adding further VLANs or larger address blocks as the office grows.

---
**Academic note:** This is a proposed design for learning purposes. No claim is made that the network has been deployed in a live office.
