# Section B: OSI and TCP/IP Models — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **26** | D | EL | 60 sec | Explain the seven layers of the OSI model. |
| **27** | D | EL | 45 sec | What is the purpose of the Physical layer? |
| **28** | D | EL | 45 sec | What does the Data Link layer do? |
| **29** | D | EL | 45 sec | What does the Network layer do? |
| **30** | D | EL | 45 sec | What does the Transport layer do? |
| **31** | D | EL | 45 sec | What does the Application layer do? |
| **32** | D | EL | 60 sec | Which OSI layers are associated with a switch and router? |
| **33** | D | EL | 60 sec | What is the TCP/IP model? |
| **34** | D | EL | 60 sec | How does the TCP/IP model differ from the OSI model? |
| **35** | D | EL | 60 sec | What is encapsulation? |
| **36** | D | EL | 60 sec | What is decapsulation? |
| **37** | S | EL | 90 sec | At which layers would you investigate a failed fiber link? |
| **38** | S | EL | 90 sec | At which layers would you investigate a wrong VLAN? |
| **39** | S | EL | 90 sec | A user has an IP address but cannot reach another network. Which layers would you check? |
| **40** | N | EL | 90 sec | Explain the OSI model using a parcel-delivery example. |

---

### Answers

#### 26. Explain the seven layers of the OSI model. [D | EL]
* **Layer 1 (Physical):** Transmits raw bitstreams over physical media like copper, fiber, or wireless.
* **Layer 2 (Data Link):** Handles node-to-node frame transfer, error detection, and MAC addressing.
* **Layer 3 (Network):** Handles logical addressing (IP) and routing packets between different subnets.
* **Layer 4 (Transport):** Provides end-to-end communication, segmentation, and reliability via TCP or UDP.
* **Layer 5 (Session):** Establishes, manages, and terminates application sessions.
* **Layer 6 (Presentation):** Handles data formatting, translation, encryption, and compression.
* **Layer 7 (Application):** Serves as the direct interface for network services to end-user software.

#### 27. What is the purpose of the Physical layer? [D | EL]
* **Definition:** Layer 1 is responsible for sending raw binary bits (1s and 0s) over physical hardware mediums.
* **Key Components:** Cabling (UTP/Fiber), voltage levels, pinouts, RF frequencies, transceiver modules (SFPs), and network interfaces.
* **Verification:** Check physical link status LEDs, verify fiber power levels using `show interfaces transceiver`, or test continuity with a cable tester.

#### 28. What does the Data Link layer do? [D | EL]
* **Definition:** Layer 2 structures raw bits into **frames** for node-to-node transmission within the same local network.
* **Key Functions:** Uses MAC addresses for local delivery, detects frame errors using Frame Check Sequence (FCS), and enforces VLAN tagging (802.1Q).
* **Verification:** Check Layer 2 switch operational status using `show mac address-table` or inspect interface statistics for CRC/FCS error counters.

#### 29. What does the Network layer do? [D | EL]
* **Definition:** Layer 3 manages logical addressing and optimal path selection (routing) across interconnected networks.
* **Key Protocols:** IPv4, IPv6, ICMP, and routing protocols like OSPF or BGP.
* **Verification:** Test path reachability with `ping` and `traceroute`, or inspect active paths using `show ip route`.

#### 30. What does the Transport layer do? [D | EL]
* **Definition:** Layer 4 establishes end-to-end communication channels and multiplexes traffic using source and destination port numbers.
* **Key Protocols:** TCP (connection-oriented, reliable, windowing) and UDP (connectionless, fast, lightweight).
* **Verification:** Inspect active listening ports and connections using `netstat -an` or test connectivity via `telnet <IP> <Port>`.

#### 31. What does the Application layer do? [D | EL]
* **Definition:** Layer 7 provides protocols that application software uses directly to access network capabilities.
* **Examples:** HTTP/HTTPS (web traffic), DNS (domain resolution), SSH (remote management), and SMTP (email).
* **Verification:** Test application-level delivery using `curl -I <URL>` or query domain records using `nslookup` / `dig`.

#### 32. Which OSI layers are associated with a switch and router? [D | EL]
* **Standard Switch:** Works at **Layer 2 (Data Link)** using destination MAC addresses to forward frames. *(Note: Layer 3 switches also perform Layer 3 routing capabilities).*
* **Router:** Works at **Layer 3 (Network)** using destination IP addresses to route packets across subnets.

#### 33. What is the TCP/IP model? [D | EL]
* **Definition:** The practical 4-layer networking protocol suite that forms the operational foundation of the modern Internet.
* **Layers:**
  1. **Network Access / Link:** (Combines OSI Layers 1 & 2)
  2. **Internet:** (Maps to OSI Layer 3)
  3. **Transport:** (Maps to OSI Layer 4)
  4. **Application:** (Combines OSI Layers 5, 6, & 7)

#### 34. How does the TCP/IP model differ from the OSI model? [D | EL]
* **Structure:** TCP/IP uses **4 layers**, whereas OSI uses **7 layers**.
* **Layer Mapping:** TCP/IP merges OSI's Session, Presentation, and Application layers into a single Application layer, and combines OSI's Physical and Data Link layers into a single Network Access layer.
* **Implementation:** OSI is a theoretical reference framework, while TCP/IP is the actual protocol implementation used on real-world networks.

#### 35. What is encapsulation? [D | EL]
* **Definition:** The process of wrapping data with protocol headers (and a Layer 2 trailer) as it moves down through the stack from Layer 7 to Layer 1.
* **PDU Workflow:** Data (L7–L5) $\rightarrow$ **Segment** (L4: TCP/UDP Header) $\rightarrow$ **Packet** (L3: IP Header) $\rightarrow$ **Frame** (L2: MAC Header & FCS) $\rightarrow$ **Bits** (L1: Physical Transmission).

#### 36. What is decapsulation? [D | EL]
* **Definition:** The reverse process of encapsulation that occurs at the receiving host as data moves up from Layer 1 to Layer 7.
* **Process:** Each layer reads its specific control header/trailer, verifies data integrity, strips the header off, and passes the remaining payload up to the next layer.

#### 37. At which layers would you investigate a failed fiber link? [S | EL]
* **Primary Layer:** **Layer 1 (Physical)** — Inspect physical fiber cables for bends or breaks, clean LC connectors, verify optical transceiver (SFP) TX/RX optical power levels, or perform OTDR testing.
* **Secondary Layer:** **Layer 2 (Data Link)** — Verify if port status shows `down/down`, check for interface flap logs, or look for excessive CRC/framing error counts.

#### 38. At which layers would you investigate a wrong VLAN? [S | EL]
* **Primary Layer:** **Layer 2 (Data Link)** — VLANs operate at Layer 2. Check switchport mode (Access vs. Trunk), 802.1Q tagging, native VLAN assignment, or run `show vlan brief`.
* **Secondary Layer:** **Layer 3 (Network)** — Check if the connected host received an incorrect IP subnet lease from DHCP due to being placed in the wrong VLAN.

#### 39. A user has an IP address but cannot reach another network. Which layers would you check? [S | EL]
* **Layer 3 (Network):** Verify host default gateway setting, subnet mask configuration, ARP resolution for the default gateway (`arp -a`), and routing table entries on upstream routers (`show ip route`).
* **Layer 2 (Data Link):** Ensure the port is assigned to the correct VLAN and that the local switch is forwarding frames properly.
* **Layer 1 (Physical):** Confirm the host's physical link state is `up`.

#### 40. Explain the OSI model using a parcel-delivery example. [N | EL]
* **Application (L7):** You write a message on a piece of paper.
* **Presentation (L6):** You translate the message into English and encode it in a secret code.
* **Session (L5):** You call the recipient to verify they are ready to accept mail.
* **Transport (L4):** You choose certified trackable delivery with confirmation receipts (TCP).
* **Network (L3):** You write the full destination address and postal zip code on the outer envelope (IP Address).
* **Data Link (L2):** Postal workers attach local sorting barcodes to route the parcel from one mail truck to the next (MAC Addresses).
* **Physical (L1):** The delivery truck physically drives over roads to move the parcel to its destination (Cables & Signals).
