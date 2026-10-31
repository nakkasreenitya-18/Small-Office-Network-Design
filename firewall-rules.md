# Proposed Firewall and Access-Control Rules

## 1. Policy principles

The firewall should follow **least privilege**: deny traffic by default between internal security zones, then permit only the flows required for business operations.

The rules below are a conceptual policy for the proposed network. Exact syntax depends on the router/firewall vendor.

## 2. Traffic policy matrix

| Source | Destination | Service | Action | Reason |
|---|---|---|---|---|
| Staff VLAN 10 | Internet | Required web/DNS services | Allow | Normal employee access |
| Staff VLAN 10 | Server VLAN 30 | Approved application/file services only | Allow | Business access |
| Staff VLAN 10 | Management VLAN 40 | Device administration | Deny by default | Protect infrastructure |
| Guest VLAN 20 | Internet | DNS, HTTP, HTTPS | Allow | Visitor internet access |
| Guest VLAN 20 | Staff VLAN 10 | Any | Deny | Isolate guest devices |
| Guest VLAN 20 | Server VLAN 30 | Any | Deny | Protect internal services |
| Guest VLAN 20 | Management VLAN 40 | Any | Deny | Protect network equipment |
| Admin workstation | Management VLAN 40 | SSH/HTTPS only | Allow | Secure administration |
| Server VLAN 30 | Internet | Approved updates/time/email as needed | Allow narrowly | Maintenance and required services |
| Any internal VLAN | Other internal VLANs | Unlisted traffic | Deny by default | Limit lateral movement |

## 3. Rule implementation notes

1. Place specific allow rules before broad deny rules where the firewall evaluates rules top-to-bottom.
2. Allow established/related return traffic where supported by a stateful firewall.
3. Permit DHCP and DNS only to the intended DHCP/DNS services.
4. Do not expose management ports such as SSH, RDP, or switch web administration to the public internet.
5. Log denied traffic that is useful for security monitoring, but avoid excessive logging that overwhelms the device.
6. Review rules periodically and remove obsolete access.

## 4. Example logical rule order

1. Allow established and related traffic.
2. Allow administrator workstation to management VLAN on approved management ports.
3. Allow staff VLAN to approved server services.
4. Allow staff VLAN to internet according to policy.
5. Allow guest VLAN to internet DNS/HTTP/HTTPS.
6. Deny guest VLAN to all internal office subnets.
7. Deny non-admin sources to management VLAN.
8. Deny other inter-VLAN traffic by default.

## 5. Important deployment caution

This is a design-level rule set, not vendor-specific configuration. Before applying rules to a live network, confirm interface names, VLAN IDs, management access, DHCP/DNS dependencies, and recovery access. A mistake in management rules can lock administrators out of network equipment.
