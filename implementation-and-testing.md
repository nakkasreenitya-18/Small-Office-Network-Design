# Implementation Sequence and Testing Checklist

## 1. Suggested implementation sequence

This is a proposed sequence for a lab or future deployment. Do not change a live office network without the network administrator's approval.

1. Confirm the number and type of devices and document business requirements.
2. Select a router/firewall and managed switch that support VLANs and the required throughput.
3. Configure the four VLANs: 10, 20, 30, and 40.
4. Configure the IP gateway address for each VLAN.
5. Configure DHCP scopes for staff and guest clients.
6. Assign static or reserved addresses to servers and network infrastructure.
7. Configure staff and guest wireless SSIDs and map each to its VLAN.
8. Configure inter-VLAN routing and apply the firewall policy.
9. Configure DNS, NTP, and only the application services that are required.
10. Back up the final configuration and document administrative access securely.

## 2. Validation checklist

| Test | Expected result |
|---|---|
| Staff client obtains DHCP address | Address is within `192.168.10.10`–`192.168.10.60` |
| Guest client obtains DHCP address | Address is within `192.168.10.70`–`192.168.10.125` |
| Staff client reaches its gateway | Successful |
| Guest client reaches its gateway | Successful |
| Staff client resolves approved DNS names | Successful |
| Guest client resolves public DNS names | Successful |
| Guest client accesses the internet | Successful, subject to policy |
| Guest client reaches staff subnet | Blocked |
| Guest client reaches server subnet | Blocked |
| Guest client reaches management subnet | Blocked |
| Non-admin staff device accesses switch management | Blocked |
| Authorised admin accesses management interface | Successful using SSH/HTTPS |
| Server is reachable on approved application ports | Successful |
| Unapproved inter-VLAN connection | Blocked |

## 3. Useful diagnostic commands

Run these only on a computer or lab device you are authorised to use.

### Windows

```powershell
ipconfig /all
ping 192.168.10.1
nslookup example.com
tracert example.com
```

### Linux/macOS

```bash
ip addr
ip route
ping -c 4 192.168.10.1
nslookup example.com
traceroute example.com
```

A failed ping does not always mean a service is down; firewalls may block ICMP. Check the intended service and firewall policy as well.

## 4. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Incorrect subnet or DHCP scope | Verify network, gateway, mask, and DHCP range before rollout |
| Guest access to internal devices | Enforce guest isolation and test blocked paths |
| Weak Wi-Fi security | Use WPA2-AES/WPA3 and strong credentials |
| Exposed management services | Restrict management to authorised admin sources |
| Configuration loss | Keep encrypted, access-controlled backups |
| Unpatched devices | Establish a regular update process |

## 5. Final review

Before submitting this project:

- [ ] Check that all four subnet ranges are correct and non-overlapping.
- [ ] Check that each VLAN has a unique subnet and gateway.
- [ ] Confirm guest access is internet-only.
- [ ] Confirm that management access is restricted.
- [ ] Check the Mermaid diagram renders on GitHub.
- [ ] Add your name, course, and submission date if your instructor requests them.
- [ ] Do not claim that tests were performed on a live network unless you actually performed them.

## 6. Learning references

- RFC 1918 — Address Allocation for Private Internets: https://www.rfc-editor.org/rfc/rfc1918
- RFC 4632 — Classless Inter-domain Routing (CIDR): https://www.rfc-editor.org/rfc/rfc4632
- RFC 2131 — Dynamic Host Configuration Protocol: https://www.rfc-editor.org/rfc/rfc2131
- RFC 1034 — Domain Names: Concepts and Facilities: https://www.rfc-editor.org/rfc/rfc1034
- RFC 8446 — The Transport Layer Security (TLS) Protocol Version 1.3: https://www.rfc-editor.org/rfc/rfc8446
