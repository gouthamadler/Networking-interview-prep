# Subnetting

## Concept

Subnetting is the process of dividing a larger network into smaller, more manageable sub-networks (subnets). It allows efficient use of IP address space, improves network performance by reducing broadcast domains, and adds a layer of organization/security by isolating traffic between groups of devices.

## Key Terms

- **Subnet Mask:** A 32-bit number that separates the network portion from the host portion of an IP address (e.g., `255.255.255.0`)
- **CIDR Notation:** Shorthand for subnet mask, written as a slash followed by the number of network bits (e.g., `/24`)
- **Network Address:** The first address in a subnet, identifies the network itself (all host bits = 0)
- **Broadcast Address:** The last address in a subnet, used to send data to all devices in that subnet (all host bits = 1)
- **Host Address:** Any usable address between the network and broadcast address, assignable to a device

## How Subnetting Works

An IP address has two parts: **network bits** and **host bits**. The subnet mask determines where that split happens.

Example: `192.168.1.0/24`
- `/24` means the first 24 bits are the network portion, leaving 8 bits for hosts
- Subnet mask: `255.255.255.0`
- Usable host range: `192.168.1.1` – `192.168.1.254`
- Network address: `192.168.1.0`
- Broadcast address: `192.168.1.255`
- Total usable hosts: 2^8 - 2 = 254 (subtracting network and broadcast addresses)

## Common CIDR / Subnet Mask Reference

| CIDR | Subnet Mask | Total Addresses | Usable Hosts |
|------|-----------------|------------------|---------------|
| /24 | 255.255.255.0 | 256 | 254 |
| /25 | 255.255.255.128 | 128 | 126 |
| /26 | 255.255.255.192 | 64 | 62 |
| /27 | 255.255.255.224 | 32 | 30 |
| /28 | 255.255.255.240 | 16 | 14 |
| /29 | 255.255.255.248 | 8 | 6 |
| /30 | 255.255.255.252 | 4 | 2 |

## Quick Subnetting Method

1. Determine how many host bits you need (based on required number of devices)
2. Figure out how many network bits that leaves (32 - host bits)
3. Convert to CIDR notation and subnet mask
4. Calculate: network address, broadcast address, usable range, total usable hosts

**Formula:** Usable hosts = 2^(host bits) - 2

## Real-World Example

A company needs to split `192.168.10.0/24` into 4 smaller subnets for different departments (Sales, IT, HR, Finance):

- Borrow 2 bits from the host portion (2^2 = 4 subnets) → new mask is `/26`
- Resulting subnets:
 - `192.168.10.0/26` (hosts: .1–.62)
 - `192.168.10.64/26` (hosts: .65–.126)
 - `192.168.10.128/26` (hosts: .129–.190)
 - `192.168.10.192/26` (hosts: .193–.254)

Each department gets its own subnet, isolating broadcast traffic and making it easier to apply department-specific firewall/VLAN rules.

## Troubleshooting Angle

- **Device can't reach others on the "same" network?** Check if it's actually in the same subnet — mismatched subnet masks are a common misconfiguration.
- **"Limited connectivity" or wrong subnet assigned?** Check DHCP scope settings — make sure the pool matches the intended subnet.
- **Two subnets can't communicate?** That's expected unless a router or Layer 3 device is configured to route between them.
- **Running out of IPs in a subnet?** May need to re-subnet with more host bits, or implement VLSM (Variable Length Subnet Masking) for more efficient allocation.

## Common Interview Questions

1. What is subnetting and why is it used?
2. How do you calculate the number of usable hosts in a subnet?
3. What's the difference between a network address and a broadcast address?
4. Convert `/26` to a subnet mask.
5. How many usable hosts are in a `/28` subnet?
6. What is VLSM and why would you use it?
7. Given an IP and subnet mask, how would you determine the network address?
8. Why can't two devices on different subnets communicate without a router?

## Practical Scenario

*"You're given the network `10.0.0.0/24` and asked to create subnets for 4 departments, each needing up to 50 hosts. How would you subnet this?"*

- 50 hosts requires 6 host bits (2^6 - 2 = 62 usable hosts) → mask becomes `/26`
- This gives you exactly 4 subnets of `/26` each, matching the 4 departments — a clean fit for this scenario.
