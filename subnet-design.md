# Subnet Design and IPv4 Address Plan

## 1. Address block

The proposed private address block is:

- Network block: `192.168.10.0/24`
- Subnet mask: `255.255.255.0`
- Total addresses in the /24 block: 256
- Private IPv4 range: within RFC 1918 address space

The /24 block is divided using VLSM (Variable Length Subnet Masking) into four networks.

## 2. Subnet allocation

| VLAN | Purpose | Network address | Prefix | Subnet mask | Usable host range | Broadcast |
|---|---|---|---:|---|---|---|
| 10 | Staff | `192.168.10.0` | /26 | `255.255.255.192` | `192.168.10.1`–`192.168.10.62` | `192.168.10.63` |
| 20 | Guest | `192.168.10.64` | /26 | `255.255.255.192` | `192.168.10.65`–`192.168.10.126` | `192.168.10.127` |
| 30 | Servers | `192.168.10.128` | /27 | `255.255.255.224` | `192.168.10.129`–`192.168.10.158` | `192.168.10.159` |
| 40 | Management | `192.168.10.160` | /28 | `255.255.255.240` | `192.168.10.161`–`192.168.10.174` | `192.168.10.175` |

Reserved for future use: `192.168.10.176`–`192.168.10.255`.

## 3. Subnet calculations

### Staff subnet — /26

A /26 prefix leaves `32 - 26 = 6` host bits.

- Total addresses: `2^6 = 64`
- Usable host addresses: `64 - 2 = 62`
- Block size in the final octet: `256 - 192 = 64`
- Network: `.0`
- Broadcast: `.63`
- Usable range: `.1` to `.62`

### Guest subnet — /26

- Total addresses: 64
- Usable host addresses: 62
- Network: `192.168.10.64`
- Broadcast: `192.168.10.127`
- Usable range: `192.168.10.65` to `192.168.10.126`

### Server subnet — /27

A /27 prefix leaves 5 host bits.

- Total addresses: `2^5 = 32`
- Usable host addresses: `32 - 2 = 30`
- Subnet mask: `255.255.255.224`
- Block size: `256 - 224 = 32`
- Network: `.128`
- Broadcast: `.159`
- Usable range: `.129` to `.158`

### Management subnet — /28

A /28 prefix leaves 4 host bits.

- Total addresses: `2^4 = 16`
- Usable host addresses: `16 - 2 = 14`
- Subnet mask: `255.255.255.240`
- Block size: `256 - 240 = 16`
- Network: `.160`
- Broadcast: `.175`
- Usable range: `.161` to `.174`

## 4. Example static/reserved addresses

| Device/service | Example address | Notes |
|---|---|---|
| Staff VLAN gateway | `192.168.10.1` | Default gateway |
| Guest VLAN gateway | `192.168.10.65` | Default gateway |
| Server VLAN gateway | `192.168.10.129` | Default gateway |
| Internal DNS/server | `192.168.10.130` | Static or DHCP reservation |
| File/print server | `192.168.10.131` | If required |
| Management gateway | `192.168.10.161` | Default gateway |
| Managed switch | `192.168.10.162` | Management address |
| Wireless access point | `192.168.10.163` | Management address |

These are example addresses. Avoid assigning the same address to more than one device.

## 5. DHCP scope proposal

| VLAN | Suggested DHCP range | Exclusions |
|---|---|---|
| Staff | `192.168.10.10`–`192.168.10.60` | Gateway and reserved infrastructure addresses |
| Guest | `192.168.10.70`–`192.168.10.125` | Gateway |
| Servers | Prefer static/reservations | Gateway and fixed server addresses |
| Management | Prefer static/reservations | Gateway and network equipment |

A DHCP scope should also provide the correct default gateway and DNS server for its VLAN. Guest clients should receive a DNS resolver that is permitted by the guest policy.

## 6. Why use VLSM?

VLSM allows subnets of different sizes to be allocated according to expected device counts. Staff and guest networks receive /26 subnets, while the server and management networks receive smaller subnets. This avoids assigning the same large subnet to every group and leaves address space for future expansion.
