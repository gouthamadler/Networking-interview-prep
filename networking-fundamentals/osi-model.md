# OSI Model

## Concept

The OSI (Open Systems Interconnection) Model is a 7-layer conceptual framework that standardizes how different networking systems communicate with each other. It breaks the complex process of network communication into smaller, manageable layers — each with a specific job — so that hardware and software from different vendors can work together predictably. It's mainly used as a teaching and troubleshooting tool rather than something implemented literally in code (that's closer to what TCP/IP does).

## Layer-by-Layer Breakdown

### Layer 7 — Application
- **Function:** Interface between the network and the end-user application (browsers, email clients, etc.)
- **Protocols/Devices:** HTTP, HTTPS, FTP, SMTP, DNS, DHCP
- **What breaks if it fails:** The app itself can't communicate — e.g., a browser can't load a page even if the network connection is fine.

### Layer 6 — Presentation
- **Function:** Translates, encrypts, and compresses data so the application layer can use it (formatting, encryption/decryption).
- **Protocols/Devices:** SSL/TLS, JPEG, ASCII, encryption standards
- **What breaks if it fails:** Data arrives but is unreadable or improperly formatted — e.g., a corrupted image or failed SSL handshake.

### Layer 5 — Session
- **Function:** Establishes, manages, and terminates connections (sessions) between two devices.
- **Protocols/Devices:** NetBIOS, PPTP, RPC
- **What breaks if it fails:** Sessions drop unexpectedly, or multiple sessions get mixed up between applications.

### Layer 4 — Transport
- **Function:** Ensures reliable (or fast, depending on protocol) end-to-end data delivery; handles segmentation, flow control, and error checking.
- **Protocols/Devices:** TCP, UDP
- **What breaks if it fails:** Data arrives out of order, gets lost without retransmission, or connections time out.

### Layer 3 — Network
- **Function:** Handles logical addressing and routing of packets between different networks.
- **Protocols/Devices:** IP, ICMP, routers
- **What breaks if it fails:** Devices can't reach networks outside their own subnet — classic "no internet but LAN works" symptom.

### Layer 2 — Data Link
- **Function:** Handles physical addressing (MAC addresses), and moves data across the same local network segment; error detection at the frame level.
- **Protocols/Devices:** Ethernet, switches, MAC addresses, ARP
- **What breaks if it fails:** Devices on the same LAN can't talk to each other; switching/VLAN issues live here.

### Layer 1 — Physical
- **Function:** The actual physical transmission of raw bits over cables, radio waves, or fiber.
- **Protocols/Devices:** Cables, hubs, NICs, connectors, radio signals
- **What breaks if it fails:** No connectivity at all — unplugged cable, dead NIC, faulty port.

## Real-World Example: Loading a Webpage

1. **Layer 7 (Application):** You type a URL into your browser; HTTP/HTTPS request is generated.
2. **Layer 6 (Presentation):** The data is encrypted via TLS (for HTTPS).
3. **Layer 5 (Session):** A session is established between your browser and the web server.
4. **Layer 4 (Transport):** TCP breaks the request into segments and ensures reliable delivery (or QUIC/UDP in some modern cases).
5. **Layer 3 (Network):** IP addressing determines how to route the packet from your device to the server, possibly across many networks.
6. **Layer 2 (Data Link):** Within your local network, MAC addresses are used to get the packet to your router/gateway.
7. **Layer 1 (Physical):** The data travels as electrical signals, light pulses, or radio waves across the actual cable/Wi-Fi/fiber.

The server then responds, and the same process happens in reverse.

## Troubleshooting Using the OSI Model

The OSI model gives you a systematic way to isolate problems instead of guessing. A common approach is **bottom-up** or **top-down** troubleshooting:

- **No internet at all?** Start at Layer 1 — is the cable plugged in? Is Wi-Fi connected?
- **Connected to Wi-Fi but no LAN access?** Check Layer 2 — MAC/ARP issues, switch port problems.
- **LAN works but can't reach the internet?** Check Layer 3 — IP configuration, default gateway, routing.
- **Can ping an IP but websites won't load?** Check Layer 7 — DNS resolution failure.
- **Site loads slowly or drops mid-session?** Check Layer 4 — TCP retransmissions, packet loss.

This layered approach is one of the most common ways interviewers expect you to reason through a "user has no internet" scenario.

## Common Interview Questions

1. What is the OSI model and why was it created?
2. Which layer does a switch operate at? What about a router?
3. What's the difference between the OSI model and the TCP/IP model?
4. At which layer does a firewall typically operate?
5. What happens at Layer 4 that doesn't happen at Layer 3?
6. Can you explain the journey of data through all 7 layers using a real example?
7. Where does encryption (SSL/TLS) fit into the OSI model?
8. If a user reports "the internet is down," how would you use the OSI model to troubleshoot?

## Quick Reference Table

| Layer # | Name | PDU (Data Unit) | Example Protocols/Devices |
|---------|--------------|-------------|------------------------------------|
| 7 | Application | Data | HTTP, HTTPS, FTP, SMTP, DNS |
| 6 | Presentation | Data | SSL/TLS, JPEG, encryption |
| 5 | Session | Data | NetBIOS, PPTP, RPC |
| 4 | Transport | Segment | TCP, UDP |
| 3 | Network | Packet | IP, ICMP, Routers |
| 2 | Data Link | Frame | Ethernet, Switches, MAC, ARP |
| 1 | Physical | Bit | Cables, Hubs, NICs, Radio waves |
