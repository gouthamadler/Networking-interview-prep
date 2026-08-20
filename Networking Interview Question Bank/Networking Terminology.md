# Section A: Networking Terminology — Questions & Answers

| # | Type | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **1** | D | EL | 30 sec | What is a computer network? |
| **2** | D | EL | 30 sec | What is a host or endpoint? |
| **3** | D | EL | 30 sec | What is a protocol? |
| **4** | D | EL | 45 sec | What is the difference between LAN, WAN, WLAN, and PAN? |
| **5** | D | EL | 45 sec | What is the difference between bandwidth, throughput, latency, jitter, and packet loss? |
| **6** | D | EL | 30 sec | What is an ISP? |
| **7** | D | EL | 45 sec | What is the difference between a client and a server? |
| **8** | D | EL | 30 sec | What is a network interface? |
| **9** | D | EL | 30 sec | What is an Ethernet frame? |
| **10** | D | EL | 30 sec | What is an IP packet? |
| **11** | D | EL | 30 sec | What is a TCP segment? |
| **12** | D | EL | 45 sec | What is a broadcast domain? |
| **13** | D | EL | 45 sec | What is a collision domain? |
| **14** | N | EL | 60 sec | Explain the Internet to a non-technical customer. |
| **15** | N | EL | 60 sec | Explain the difference between Wi-Fi and the Internet. |
| **16** | N | EL | 60 sec | Explain latency using a real-life example. |
| **17** | D | EL | 30 sec | What is a network topology? |
| **18** | D | EL | 30 sec | What is a point-to-point connection? |
| **19** | D | EL | 30 sec | What is a client-server network? |
| **20** | N | EL | 60 sec | Explain why restarting a router sometimes fixes a connection problem. |
| **21** | S | EL | 90 sec | Explain what happens during a typical broadband installation from customer premises to Internet access. |
| **22** | D | EL | 60 sec | What is the difference between FTTH and traditional copper broadband? |
| **23** | D | EL | 60 sec | What are OLT, ONT, ONU, splitter, and ODN? |
| **24** | N | EL | 60 sec | Explain an ONT to a customer who thinks it is the same as a Wi-Fi router. |
| **25** | S | EL | 90 sec | A customer says, “My Wi-Fi is connected, but the Internet is not working.” What does that statement mean technically? |

---

### Core Questions & Answers

#### 1. What is a computer network?
* **Definition:** A collection of interconnected computing devices that communicate and share resources (data, applications, internet, printers) using shared rules and physical or wireless media.
* **Example:** A home network connecting laptops, phones, and a printer to a central Wi-Fi router.
* **Verification:** Use `ping <ip-address>` to verify reachability between two devices on the network.

#### 2. What is a host or endpoint?
* **Definition:** Any end-user device or computer system connected to a network that originates or receives traffic and possesses an IP address.
* **Example:** Laptops, smartphones, IP phones, servers, and IoT devices.
* **Verification:** Run `ipconfig` (Windows) or `ifconfig`/`ip a` (Linux/Mac) to view the endpoint's IP configuration.

#### 3. What is a protocol?
* **Definition:** A standardized set of rules and conventions that determine how data is formatted, transmitted, received, and processed across a network.
* **Example:** HTTP/HTTPS for web browsing, TCP for reliable transport, and IP for addressing and routing.
* **Verification:** Inspect network traffic using a packet analyzer like Wireshark to observe protocol headers.

#### 4. What is the difference between LAN, WAN, WLAN, and PAN?
* **PAN (Personal Area Network):** Covers a tiny area for personal devices (e.g., Bluetooth headphones connected to a phone).
* **LAN (Local Area Network):** Connects devices within a limited geographical area like an office building or home via Ethernet switches.
* **WLAN (Wireless LAN):** A LAN that utilizes wireless signals (Wi-Fi / 802.11 standard) instead of cables.
* **WAN (Wide Area Network):** Spans large geographic regions, interconnecting multiple LANs via service provider links (e.g., the Internet or MPLS).

#### 5. What is the difference between bandwidth, throughput, latency, jitter, and packet loss?
* **Bandwidth:** The theoretical maximum data capacity of a link (e.g., a 100 Mbps connection).
* **Throughput:** The actual rate of successful data delivery over that link at a given time.
* **Latency:** The delay/time taken for data to travel from source to destination (measured in milliseconds).
* **Jitter:** The variation or fluctuation in latency over time, critical for real-time traffic like VoIP.
* **Packet Loss:** The percentage of sent data packets that fail to reach their destination due to congestion or errors.

#### 6. What is an ISP?
* **Definition:** Internet Service Provider—a company that provides individuals and organizations commercial access to the global Internet.
* **Example:** Telecom providers such as Airtel, Jio, AT&T, or Comcast.
* **Verification:** Use `traceroute` or `tracert` to trace the path from a local network out through the ISP's edge routers.

#### 7. What is the difference between a client and a server?
* **Client:** A device or application that requests services, pages, or files (e.g., a web browser on a personal laptop).
* **Server:** A high-availability system that listens for requests, processes them, and delivers services or data back to clients (e.g., an Nginx web server or SQL database).

#### 8. What is a network interface?
* **Definition:** The point of interconnection between a device and a network, which can be a physical hardware component (NIC) or a software virtual adapter.
* **Example:** Ethernet port (RJ-45), Wi-Fi card, or a software Loopback interface (`127.0.0.1`).
* **Verification:** Run `getmac` or `show interfaces` on a router to check active interface states.

#### 9. What is an Ethernet frame?
* **Definition:** The Data Link layer (Layer 2) Protocol Data Unit (PDU) used for local transmission across an Ethernet network.
* **Structure:** Contains a header with Source and Destination MAC addresses, a payload, and a Frame Check Sequence (FCS) trailer for error checking.

#### 10. What is an IP packet?
* **Definition:** The Network layer (Layer 3) PDU responsible for end-to-end addressing and routing across different networks.
* **Structure:** Contains Source IP, Destination IP, TTL (Time to Live), Protocol fields, and the Layer 4 payload.

#### 11. What is a TCP segment?
* **Definition:** The Transport layer (Layer 4) PDU for the Transmission Control Protocol, providing connection-oriented, reliable delivery.
* **Structure:** Contains Source/Destination Port numbers, Sequence Numbers, Acknowledgment Numbers, Flags (SYN, ACK, FIN), and window size.

#### 12. What is a broadcast domain?
* **Definition:** A logical division of a computer network in which any device can reach all others by broadcasting at the Data Link layer.
* **Boundary:** Routers stop layer 2 broadcasts and define broadcast domain boundaries. Switches forward broadcasts across all ports within the same VLAN.

#### 13. What is a collision domain?
* **Definition:** A network segment where data packets can collide with one another when sent on a shared medium.
* **Boundary:** Switches break up collision domains—each individual port on a switch is its own collision domain. Hubs form a single shared collision domain across all ports.

#### 14. Explain the Internet to a non-technical customer.
* **Analogy:** "Think of the Internet like a global postal service. Your computer writes a letter (data), puts an address on it, and hands it to your local post office (your router and Internet Service Provider). The provider uses a global highway system of underground cables to deliver that letter to a building on the other side of the world in a fraction of a second, and then brings back their response."

#### 15. Explain the difference between Wi-Fi and the Internet.
* **Analogy:** "Wi-Fi is like a cordless landline phone inside your home, while the Internet is the actual telephone network connecting your house to the outside world. Wi-Fi simply connects your phone or laptop wirelessly to your home router. The Internet is the service flowing through the fiber cable outside into that router."

#### 16. Explain latency using a real-life example.
* **Analogy:** "Latency is the delay before a transfer begins. Imagine ordering food: if you call a restaurant and they take 5 seconds to answer the phone before you can speak, that 5-second pause is latency. High latency in voice or video calls causes people to talk over each other because of the lag."

#### 17. What is a network topology?
* **Definition:** The structural arrangement or layout of network nodes and connecting links.
* **Types:** Physical topology (cabling structure like Star, Mesh, Bus, Ring) and Logical topology (how data flows through the network).

#### 18. What is a point-to-point connection?
* **Definition:** A dedicated communications link established directly between exactly two network nodes or endpoints.
* **Example:** A direct serial link connecting two enterprise routers, or a point-to-point wireless bridge linking two adjacent office buildings.

#### 19. What is a client-server network?
* **Definition:** A centralized network architecture where client devices rely on dedicated, centralized server hosts for resources, processing, authentication, and data storage.
* **Contrast:** Differs from Peer-to-Peer (P2P) networks, where every device acts as both client and server sharing resources equally.

#### 20. Explain why restarting a router sometimes fixes a connection problem.
* **Explanation:** Over time, a router's internal memory (RAM) can become cluttered with stale routing entries, frozen processes, unreleased DHCP leases, or memory leaks. Unplugging the router clears its temporary RAM, clears stale state tables, forces it to re-establish a fresh connection with the ISP, and re-initializes hardware interfaces cleanly.

---

### Profile-Specific Follow-ups

#### 21. Explain what happens during a typical broadband installation from customer premises to Internet access.
* **Physical Deployment:** Drop fiber cable is routed from the outdoor Fiber Distribution Management System (FDMS) box or pole splitter to the customer premises using an optical rosette box.
* **Customer Premises Connection:** Fiber attaches to the Optical Network Terminal (ONT).
* **Provisioning & Authentication:** The technician registers the ONT's Serial Number (SLID/FSAN) on the Optical Line Terminal (OLT). The WAN configuration applies automatically or manually via PPPoE credentials or IPoE (DHCP).
* **Service Verification:** The router obtains a public/WAN IP address, tests optical power levels (RX optical power should typically be between -18 dBm to -25 dBm), and verifies DNS resolution and throughput.

#### 22. What is the difference between FTTH and traditional copper broadband?
* **FTTH (Fiber to the Home):** Uses optical fiber strands transmitting light pulses directly to the subscriber premises. Offers immense bandwidth, symmetric upload/download speeds, immune to electromagnetic interference, and operates reliably over long distances.
* **Copper Broadband (DSL/Cable):** Uses copper telephone lines or coaxial cables sending electrical signals. Highly susceptible to signal attenuation over distance, crosstalk, weather degradation, and offers lower speed limits.

#### 23. What are OLT, ONT, ONU, splitter, and ODN?
* **OLT (Optical Line Terminal):** The ISP's central office hardware that anchors the Passive Optical Network (PON).
* **ONT (Optical Network Terminal):** The end-user device located at the customer premises converting optical light signals into Ethernet/electrical signals.
* **ONU (Optical Network Unit):** Similar to an ONT, typically located at a curb or building basement serving multiple premises.
* **Splitter:** A passive, non-powered optical device that splits a single fiber strand into multiple downstream sub-branches (e.g., 1:32 or 1:64 split).
* **ODN (Optical Distribution Network):** The physical fiber infrastructure, splitters, splice trays, and connectors connecting the OLT to the ONTs.

#### 24. Explain an ONT to a customer who thinks it is the same as a Wi-Fi router.
* **Explanation:** "Think of the ONT as a universal signal translator and the Wi-Fi router as a local distribution manager. The ONT converts light signals coming through the glass fiber cable from the street into standard electrical network signals. The Wi-Fi router then takes those electrical signals and broadcasts them wirelessly to your phones and laptops throughout the house."

#### 25. A customer says, “My Wi-Fi is connected, but the Internet is not working.” What does that statement mean technically?
* **Technical Breakdown:** Layer 1/Layer 2 connectivity between the customer device and the local Wi-Fi Access Point/Router is established successfully, but Layer 3 WAN reachability or IP service is failing beyond the LAN interface.
* **Possible Causes:**
  1. Router failed to obtain a Public IP from the ISP (PPPoE authentication error or DHCP timeout).
  2. Loss of fiber/DSL physical link at the WAN port (Optical loss / LOS red light).
  3. DNS server failure (Device can reach IP addresses like `8.8.8.8` but cannot resolve domain names like `google.com`).
  4. Default gateway or IP misconfiguration on the local host.
