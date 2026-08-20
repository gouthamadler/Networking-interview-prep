# Section C: MAC Addresses, Framing, and Switching Fundamentals — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **41** | D | EL | 45 sec | What is a MAC address? |
| **42** | D | EL | 45 sec | What is an IP address? |
| **43** | D | EL | 60 sec | Explain MAC address versus IP address. |
| **44** | D | EL | 45 sec | What is ARP? |
| **45** | S | EL | 90 sec | What happens when a host wants to send traffic to another local host? |
| **46** | D | EL | 45 sec | What is a unicast frame? |
| **47** | D | EL | 45 sec | What is a broadcast frame? |
| **48** | D | EL | 45 sec | What is a multicast frame? |
| **49** | D | EL | 60 sec | How does a switch learn MAC addresses? |
| **50** | D | EL | 60 sec | What happens when a switch receives a frame for an unknown destination MAC? |
| **51** | T | EL | 90 sec | How would you investigate an ARP-related connectivity issue? |
| **52** | T | EL | 90 sec | A switch has learned a MAC address on the wrong port. What could cause this? |
| **53** | D | EL | 45 sec | What is MAC address aging? |
| **54** | D | ML | 60 sec | What is a MAC address table overflow attack? |
| **55** | D | ML | 60 sec | What is port security? |

---

### Answers

#### 41. What is a MAC address? [D | EL]
* **Definition:** A Media Access Control (MAC) address is a unique 48-bit (6-byte) physical hardware address burned into a Network Interface Card (NIC) at the factory.
* **Format:** Written in hexadecimal (e.g., `00:1A:2B:3C:4D:5E`). The first 24 bits represent the Organizationally Unique Identifier (OUI/vendor), and the last 24 bits represent the vendor-assigned NIC interface ID.
* **Verification:** Run `getmac` or `ipconfig /all` on Windows, or `ip link` on Linux.

#### 42. What is an IP address? [D | EL]
* **Definition:** A logical network address assigned to a device to identify its location on a Layer 3 network and enable routing across different subnets.
* **Types:** IPv4 (32-bit dotted-decimal, e.g., `192.168.1.1`) and IPv6 (128-bit hexadecimal, e.g., `2001:db8::1`).
* **Verification:** Run `ipconfig` on Windows or `ip addr` on Linux.

#### 43. Explain MAC address versus IP address. [D | EL]
* **MAC Address (Layer 2):** Physical, hardcoded, non-routable address used strictly for local delivery within the same broadcast domain/LAN segment.
* **IP Address (Layer 3):** Logical, configurable, routable address used to route data across different networks globally.
* **Analogy:** A MAC address is like a person's Social Security or National ID number (permanent identity); an IP address is like their current mailing address (changes when moving locations).

#### 44. What is ARP? [D | EL]
* **Definition:** Address Resolution Protocol (ARP) is a Layer 2/3 protocol used to dynamically resolve a known Layer 3 IP address into an unknown Layer 2 MAC address on a local segment.
* **Process:** The source sends an ARP Request broadcast (`Who has 192.168.1.1?`), and the target device responds with a unicast ARP Reply (`192.168.1.1 is at 00:1A:2B:3C:4D:5E`).
* **Verification:** View the local ARP table using `arp -a`.

#### 45. What happens when a host wants to send traffic to another local host? [S | EL]
1. **IP Check:** The source checks its subnet mask and determines the destination host is on the same local network segment.
2. **ARP Lookup:** The source checks its local ARP cache for the destination's MAC address.
3. **ARP Resolution (if needed):** If missing, it broadcasts an ARP Request (`FF:FF:FF:FF:FF:FF`). The target host replies with its unicast MAC.
4. **Frame Construction:** The source encapsulates the packet into an Ethernet frame with the target's MAC address as the destination.
5. **Switch Forwarding:** The local switch receives the frame, inspects the destination MAC, looks up its CAM table, and forwards the frame out the specific port connected to the target.

#### 46. What is a unicast frame? [D | EL]
* **Definition:** A frame addressed to one specific destination network interface (one-to-one communication).
* **Destination Address:** A individual host MAC address (e.g., `00:11:22:33:44:55`).
* **Switch Behavior:** The switch forwards the frame exclusively out the single port associated with that MAC address in its CAM table.

#### 47. What is a broadcast frame? [D | EL]
* **Definition:** A frame intended for all devices within the local broadcast domain/VLAN (one-to-all communication).
* **Destination Address:** `FF:FF:FF:FF:FF:FF`.
* **Switch Behavior:** The switch floods the frame out all active ports belonging to that VLAN, except the receiving port.

#### 48. What is a multicast frame? [D | EL]
* **Definition:** A frame intended for a specific group of interested devices on a network (one-to-many communication).
* **Destination Address:** Starts with `01:00:5E` for IPv4 multicast (e.g., OSPF, RIP updates).
* **Switch Behavior:** Flooded out all ports by default, unless **IGMP Snooping** is enabled to forward traffic only to registered receiver ports.

#### 49. How does a switch learn MAC addresses? [D | EL]
* **Mechanism:** A Layer 2 switch continuously inspects the **Source MAC Address** of every incoming frame on every port.
* **Learning Step:** If the source MAC is not present in its Content Addressable Memory (CAM) table, the switch binds that MAC address to the receiving port number along with a timestamp and VLAN ID.
* **Update Step:** If the entry already exists on that port, the switch refreshes its aging timer.

#### 50. What happens when a switch receives a frame for an unknown destination MAC? [D | EL]
* **Action:** The switch performs **Unknown Unicast Flooding**.
* **Process:** Because the destination MAC address is not listed in its CAM table, the switch forwards a copy of the frame out all ports in the same VLAN, except the port on which the frame arrived. Once the target host replies, the switch learns its port and resumes normal unicast forwarding.

#### 51. How would you investigate an ARP-related connectivity issue? [T | EL]
1. **Check Local ARP Table:** Run `arp -a` to see if the target IP resolves to the correct MAC, or shows incomplete/incorrect entries.
2. **Clear ARP Cache:** Clear stale entries using `arp -d *` (Windows) or `ip neigh flush all` (Linux) and ping the target again to trigger a fresh request.
3. **Packet Capture:** Use Wireshark or `tcpdump` to verify if ARP requests are being transmitted and if ARP responses are coming back.
4. **Check for Duplicate IPs / Poisoning:** Look for flapping MAC-to-IP bindings or gratuitous ARP anomalies on upstream switches.

#### 52. A switch has learned a MAC address on the wrong port. What could cause this? [T | EL]
* **Layer 2 Loop:** A switching loop caused by lack of Spanning Tree Protocol (STP) or a misconfigured bridge circulating frames continuously.
* **MAC Spoofing / Attack:** A malicious host or testing tool generating traffic forged with another host's MAC address (ARP spoofing / man-in-the-middle).
* **Physical Topology Change / Roaming:** A device (or VM) was physically re-plugged or migrated (vMotion) to a new switchport without clearing old CAM entries.
* **Duplicate MAC Addresses:** Two NICs on the network accidentally sharing the same MAC address.

#### 53. What is MAC address aging? [D | EL]
* **Definition:** A switch mechanism that automatically purges inactive MAC table entries after a specified period of inactivity to free up CAM memory.
* **Default Timer:** Usually **300 seconds (5 minutes)** on Cisco switches.
* **Process:** If no frames arrive with that source MAC within the timer window, the entry is dropped from the table.

#### 54. What is a MAC address table overflow attack? [D | ML]
* **Definition:** A Cyber/L2 attack (often using tools like `macof`) where an attacker floods a switch with thousands of fake random source MAC addresses.
* **Impact:** The switch's CAM table fills up to maximum capacity.
* **Result:** The switch enters "fail-open" mode, treating all incoming unicast traffic as unknown unicast and flooding frames out all ports, allowing the attacker to sniff private network traffic.

#### 55. What is port security? [D | ML]
* **Definition:** A Layer 2 security feature on managed switches that restricts port access to authorized MAC addresses only.
* **Key Features:**
  * Limits the maximum number of learned MAC addresses on an interface (prevents CAM overflow attacks).
  * **Static / Sticky MAC:** Permanently locks specific MAC addresses to a port.
  * **Violation Modes:** Configurable actions when an unauthorized device connects: `Protect` (drops packets), `Restrict` (drops packets & logs alert), or `Shutdown` (puts interface into `err-disable` state).
