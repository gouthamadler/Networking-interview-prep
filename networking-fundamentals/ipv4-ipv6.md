# IPv4 & IPv6

## Concept

IP (Internet Protocol) addressing is how devices on a network are uniquely identified so data can be routed to the correct destination. IPv4 is the original, widely-used addressing scheme, while IPv6 was developed to solve IPv4's address exhaustion problem and improve efficiency. Both operate at the Internet layer (TCP/IP) / Network layer (OSI).

## IPv4

- **Format:** 32-bit address, written as 4 decimal octets separated by dots (e.g., `192.168.1.1`)
- **Address space:** ~4.3 billion addresses total
- **Address classes:** A, B, C, D (multicast), E (experimental) — though classless addressing (CIDR) is standard practice today
- **Private ranges (RFC 1918):**
 - `10.0.0.0 – 10.255.255.255`
 - `172.16.0.0 – 172.31.255.255`
 - `192.168.0.0 – 192.168.255.255`
- **Key limitation:** Address exhaustion — the world has run out of new IPv4 addresses to allocate, which is why NAT and IPv6 exist.

## IPv6

- **Format:** 128-bit address, written as 8 groups of 4 hexadecimal digits separated by colons (e.g., `2001:0db8:85a3:0000:0000:8a2e:0370:7334`)
- **Address space:** ~340 undecillion addresses (practically unlimited)
- **Shorthand rules:**
 - Leading zeros in a group can be omitted (`0db8` → `db8`)
 - One consecutive run of all-zero groups can be replaced with `::` (only once per address)
- **No need for NAT:** Because address space is so large, every device can theoretically have a public, unique address.
- **Built-in features:** Simplified header, no broadcast (uses multicast/anycast instead), built-in support for auto-configuration (SLAAC) and IPsec.

## IPv4 vs IPv6 — Key Differences

| Aspect | IPv4 | IPv6 |
|---|---|---|
| Address length | 32-bit | 128-bit |
| Address format | Decimal, dotted | Hexadecimal, colon-separated |
| Address space | ~4.3 billion | ~340 undecillion |
| NAT requirement | Common (due to shortage) | Generally not needed |
| Header complexity | More complex | Simplified |
| Broadcast | Supported | Not supported (uses multicast) |
| Configuration | Manual or DHCP | DHCPv6 or SLAAC (auto-config) |
| Security | Optional (IPsec add-on) | Built-in support for IPsec |

## Real-World Example

When your laptop connects to Wi-Fi, it's commonly assigned both an IPv4 address (e.g., `192.168.1.45`) via DHCP and an IPv6 address (e.g., `2001:db8::1a2b`) via SLAAC or DHCPv6 — many modern networks run "dual stack," using both protocols simultaneously during the transition period from IPv4 to IPv6.

## Troubleshooting Angle

- **Device has no IP address at all?** Check DHCP — is the DHCP server reachable, is the lease pool exhausted?
- **Device has a `169.254.x.x` address?** That's APIPA — indicates the device couldn't reach a DHCP server and self-assigned an address (IPv4 only).
- **IPv6 connectivity issues but IPv4 works fine?** Common in dual-stack environments — check if IPv6 is properly enabled/routed on the network, since some ISPs/routers only partially support it.
- **Two devices with the same IP address?** IP conflict — causes intermittent connectivity for both devices.

## Common Interview Questions

1. What's the difference between IPv4 and IPv6?
2. Why was IPv6 introduced?
3. How many bits are in an IPv4 address? An IPv6 address?
4. What is NAT and why is it more commonly needed with IPv4?
5. What are the private IP ranges defined in RFC 1918?
6. What does `::` mean in an IPv6 address?
7. What is dual-stack networking?
8. What is APIPA and when would you see it?

## Quick Reference Table

| Feature | IPv4 | IPv6 |
|---|---|---|
| Length | 32-bit | 128-bit |
| Example | 192.168.1.1 | 2001:db8::1 |
| Total addresses | ~4.3 billion | ~340 undecillion |
| Private range example | 192.168.0.0/16 | fc00::/7 (unique local) |
| Loopback | 127.0.0.1 | ::1 |
