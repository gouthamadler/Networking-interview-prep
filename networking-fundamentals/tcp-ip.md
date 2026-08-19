# TCP/IP Model

## Concept

The TCP/IP Model (also called the Internet Protocol Suite) is the practical, real-world framework that actually runs the internet — unlike the OSI model, which is mostly theoretical/teaching-focused. It condenses OSI's 7 layers into 4 layers and is the model routers, computers, and servers actually implement in their networking stacks.

## Layer-by-Layer Breakdown

### Layer 4 — Application
- **Function:** Combines OSI's Application, Presentation, and Session layers. Handles user-facing protocols and data formatting.
- **Protocols:** HTTP, HTTPS, FTP, SMTP, DNS, DHCP, Telnet, SSH
- **What breaks if it fails:** The specific service/app can't function — e.g., DNS fails, email won't send, website won't load — even though lower-level connectivity is fine.

### Layer 3 — Transport
- **Function:** Same role as OSI Layer 4 — reliable or fast end-to-end delivery, segmentation, flow control.
- **Protocols:** TCP, UDP
- **What breaks if it fails:** Data loss without retransmission, connection timeouts, out-of-order delivery.

### Layer 2 — Internet
- **Function:** Equivalent to OSI Layer 3 — logical addressing and routing packets across networks.
- **Protocols:** IP (IPv4/IPv6), ICMP, ARP
- **What breaks if it fails:** Packets can't be routed to the correct destination network; "unreachable" errors.

### Layer 1 — Network Access (Link)
- **Function:** Combines OSI's Data Link and Physical layers — handles the physical transmission and local addressing.
- **Protocols/Devices:** Ethernet, Wi-Fi, MAC addressing, switches, cables, NICs
- **What breaks if it fails:** No local connectivity at all — cable unplugged, NIC failure, Wi-Fi signal issues.

## OSI vs TCP/IP — Key Differences

| Aspect | OSI Model | TCP/IP Model |
|---|---|---|
| Layers | 7 | 4 |
| Nature | Theoretical/reference model | Practical, implemented model |
| Development | Developed by ISO | Developed by DoD/ARPANET, predates OSI |
| Usage | Used for teaching and troubleshooting | Used to actually run the internet |
| Layer mapping | Session, Presentation are separate | Combined into Application layer |

## Real-World Example: Sending an Email

1. **Application Layer:** You compose and hit send in your email client — SMTP protocol formats the request.
2. **Transport Layer:** TCP breaks the message into segments and ensures reliable delivery to the mail server.
3. **Internet Layer:** IP addressing determines the path from your device to the destination mail server, hopping across networks.
4. **Network Access Layer:** Your local network hardware (NIC, switch, Wi-Fi) physically transmits the data as electrical/radio/light signals.

## Troubleshooting Using TCP/IP

- **No connectivity at all?** Check Network Access layer — cable, Wi-Fi, NIC.
- **Connected locally but can't reach other networks?** Check Internet layer — IP config, default gateway, subnet mask.
- **Can reach an IP but service is slow/dropping?** Check Transport layer — TCP retransmissions, packet loss, port blocking.
- **Network fully up but a specific service fails?** Check Application layer — DNS resolution, service down, firewall blocking a specific port.

This is the model most real-world engineers actually reference day to day (rather than the full 7-layer OSI breakdown), since it maps more directly to what you configure and troubleshoot.

## Common Interview Questions

1. What's the difference between the OSI model and TCP/IP model?
2. Why does TCP/IP have 4 layers while OSI has 7?
3. Which OSI layers map to the TCP/IP Application layer?
4. What layer does IP operate at in the TCP/IP model?
5. How would you troubleshoot "server unreachable" using the TCP/IP model?
6. Is TCP/IP a theoretical model or an implemented one? Why does that matter?
7. Where does ARP fit into the TCP/IP model?

## Quick Reference Table

| Layer # | Name | Equivalent OSI Layers | Example Protocols |
|---------|------------------|----------------------------------|----------------------------|
| 4 | Application | Application, Presentation, Session | HTTP, HTTPS, FTP, SMTP, DNS, DHCP |
| 3 | Transport | Transport | TCP, UDP |
| 2 | Internet | Network | IP, ICMP, ARP |
| 1 | Network Access | Data Link, Physical | Ethernet, Wi-Fi, MAC, NICs |
