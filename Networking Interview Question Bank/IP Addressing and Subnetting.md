# Section D: IP Addressing and Subnetting — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **56** | D | EL | 45 sec | What is IPv4? |
| **57** | D | EL | 45 sec | What is a subnet mask? |
| **58** | D | EL | 60 sec | What is CIDR notation? |
| **59** | D | EL | 45 sec | What is a network address? |
| **60** | D | EL | 45 sec | What is a broadcast address? |
| **61** | D | EL | 45 sec | What is a usable host address? |
| **62** | D | EL | 60 sec | What is a default gateway? |
| **63** | D | EL | 60 sec | What are private and public IP addresses? |
| **64** | D | EL | 45 sec | What is APIPA? |
| **65** | S | EL | 60 sec | A user has a 169.254.x.x address. What does it suggest? |
| **66** | D | EL | 60 sec | What is the difference between static and dynamic IP addressing? |
| **67** | D | EL | 45 sec | What is DHCP reservation? |
| **68** | D | EL | 45 sec | What is a loopback address? |
| **69** | D | EL | 45 sec | What is IPv6 and why is it needed? |
| **70** | D | EL | 45 sec | What is the difference between IPv4 and IPv6? |
| **71** | D | EL | 60 sec | What is a link-local IPv6 address? |
| **72** | D | EL | 60 sec | What is a default route? |
| **73** | T | EL | 90 sec | How would you troubleshoot an IP address conflict? |
| **74** | T | EL | 90 sec | A device can reach local hosts but not remote networks. What do you check? |
| **75** | N | EL | 60 sec | Explain subnetting to a non-technical person. |
| **76** | D | EL | 90 sec | How many usable hosts are available in a /24 network? |
| **77** | D | EL | 90 sec | How many usable hosts are available in a /26 network? |
| **78** | D | EL | 90 sec | What is the subnet mask for /25, /26, /27, /28, and /30? |
| **79** | S | EL | 120 sec | Find the network address, broadcast address, and usable range for 192.168.10.67/26. |
| **80** | S | EL | 120 sec | Are 192.168.1.10/24 and 192.168.2.10/24 in the same subnet? |
| **81** | S | EL | 120 sec | Divide 192.168.1.0/24 into four equal subnets. |
| **82** | S | EL | 120 sec | You need 50 hosts in each subnet. Which prefix would you select? |
| **83** | S | EL | 120 sec | You need point-to-point links between routers. Which subnet size is commonly suitable? |
| **84** | S | EL | 120 sec | A customer has IP 192.168.1.130/25 and gateway 192.168.1.1. Is the gateway valid for that host? |
| **85** | S | EL | 120 sec | A user’s IP is correct, but the subnet mask is /16 instead of /24. What problems could occur? |

---

### Core Questions & Answers

#### 56. What is IPv4? [D | EL]
* **Definition:** Internet Protocol Version 4 is a 32-bit logical address scheme used to identify network interfaces on a network.
* **Format:** Written as four decimal octets separated by dots (e.g., `192.168.1.1`), providing a total address space of roughly 4.3 billion addresses.

#### 57. What is a subnet mask? [D | EL]
* **Definition:** A 32-bit number that segregates an IP address into two distinct parts: the network portion (represented by continuous binary `1`s) and the host portion (represented by continuous binary `0`s).
* **Example:** `255.255.255.0` indicates that the first 24 bits define the network and the last 8 bits define local endpoints.

#### 58. What is CIDR notation? [D | EL]
* **Definition:** Classless Inter-Domain Routing (CIDR) is a compact notation for representing subnet masks by appending a forward slash `/` followed by the number of network bits.
* **Example:** `/24` equals `255.255.255.0` (24 active network bits).

#### 59. What is a network address? [D | EL]
* **Definition:** The very first IP address in a subnet range where all host bits are binary `0`. 
* **Usage:** It identifies the subnet itself and cannot be assigned to any individual host device.

#### 60. What is a broadcast address? [D | EL]
* **Definition:** The final IP address in a subnet range where all host bits are binary `1`.
* **Usage:** Used to deliver traffic simultaneously to all endpoints within that specific subnet (e.g., `192.168.1.255` on a `/24`).

#### 61. What is a usable host address? [D | EL]
* **Definition:** Any valid IP address within a subnet range that sits between the network address and the broadcast address.
* **Formula:** The number of usable host addresses is calculated using $2^h - 2$, where $h$ is the number of available host bits (subtracting 2 for the Network and Broadcast addresses).

#### 62. What is a default gateway? [D | EL]
* **Definition:** The Layer 3 router interface IP address on a local subnet that host devices forward traffic to when sending data outside their own local network.
* **Verification:** Displayed using `ipconfig` (Windows) or `ip route` (Linux).

#### 63. What are private and public IP addresses? [D | EL]
* **Private IP Addresses:** Reserved by RFC 1918 for internal LAN use; non-routable over the public Internet.
  * `10.0.0.0` – `10.255.255.255` (`10.0.0.0/8`)
  * `172.16.0.0` – `172.31.255.255` (`172.16.0.0/12`)
  * `192.168.0.0` – `192.168.255.255` (`192.168.0.0/16`)
* **Public IP Addresses:** Globally unique addresses assigned by IANA/ISPs, used to route traffic across the public Internet.

#### 64. What is APIPA? [D | EL]
* **Definition:** Automatic Private IP Addressing is a feature in client operating systems (like Windows) that automatically assigns a link-local IPv4 address when a DHCP server cannot be reached.
* **Range:** `169.254.0.1` to `169.254.255.254` with subnet mask `255.255.0.0` (`/16`).

#### 65. A user has a 169.254.x.x address. What does it suggest? [S | EL]
* **Diagnosis:** The host failed to obtain a dynamic IP configuration from a DHCP server.
* **Causes:** Physical link failure, disconnected network cable, incorrect VLAN assignment, or the local DHCP server/relay agent being offline or unreachable.

#### 66. What is the difference between static and dynamic IP addressing? [D | EL]
* **Static IP:** Manually configured directly on the device interface; remains fixed unless manually altered. Used for servers, printers, network switches, and gateways.
* **Dynamic IP:** Automatically assigned to clients by a DHCP server for a limited lease duration; efficient for managing large numbers of end-user endpoints.

#### 67. What is DHCP reservation? [D | EL]
* **Definition:** A rule created on a DHCP server that explicitly binds a host's unique Layer 2 MAC address to a specific Layer 3 IP address.
* **Benefit:** The endpoint automatically receives the same IP address dynamically via DHCP every time it boots up.

#### 68. What is a loopback address? [D | EL]
* **Definition:** A special virtual IPv4 address (`127.0.0.1`) or IPv6 address (`::1`) used by a host to send network traffic back to itself.
* **Purpose:** Allows local verification of the host's TCP/IP software stack function without transmitting frames over a physical network wire.

#### 69. What is IPv6 and why is it needed? [D | EL]
* **Definition:** Next-generation Internet Protocol using 128-bit addresses (represented as 8 hexadecimal blocks separated by colons).
* **Purpose:** Developed to solve the global exhaustion of 32-bit IPv4 addresses, providing $2^{128}$ total available addresses while eliminating the strict requirement for NAT.

#### 70. What is the difference between IPv4 and IPv6? [D | EL]
* **Address Size:** IPv4 is 32-bit (4.3 billion IPs); IPv6 is 128-bit ($3.4 \times 10^{38}$ IPs).
* **Format:** IPv4 uses dotted-decimal (`192.168.1.1`); IPv6 uses hexadecimal (`2001:db8::1`).
* **Configuration:** IPv4 relies heavily on manual setup or DHCP; IPv6 supports Stateless Address Autoconfiguration (SLAAC) out of the box.
* **Broadcast:** IPv4 uses broadcast; IPv6 eliminates broadcasts in favor of targeted multicast and ICMPv6.

#### 71. What is a link-local IPv6 address? [D | EL]
* **Definition:** An IPv6 address used exclusively for communicating with neighboring endpoints on the same local physical link/segment.
* **Prefix:** Always begins with `fe80::/10`. They are automatically generated on every IPv6-enabled interface and are non-routable past a Layer 3 router boundary.

#### 72. What is a default route? [D | EL]
* **Definition:** A catch-all entry in a Layer 3 routing table used to forward packets destined for networks not explicitly matched by any other specific route.
* **Notation:** Represented as `0.0.0.0/0` in IPv4 and `::/0` in IPv6.

#### 73. How would you troubleshoot an IP address conflict? [T | EL]
1. **Identify Duplicate:** Observe OS error pop-ups, check system event logs, or look for flapping MAC entries in switch logs.
2. **Isolate Duplicate Host:** Disconnect the affected device and attempt to `ping` the IP. If it still responds, clear the ARP cache (`arp -d`) and run `arp -a` to capture the MAC address of the conflicting host.
3. **Locate Device:** Check switch CAM tables (`show mac address-table address <MAC>`) to identify the exact physical port where the rogue device is connected.
4. **Remediate:** Convert one host to use DHCP or update its static configuration to an unallocated IP address.

#### 74. A device can reach local hosts but not remote networks. What do you check? [T | EL]
1. **Default Gateway Configuration:** Verify that the host has a valid default gateway IP assigned, and that it resides within the local subnet.
2. **Gateway Reachability:** Run `ping <default-gateway-IP>` to ensure Layer 3 communication to the local router.
3. **Subnet Mask:** Ensure the subnet mask on the client is correctly entered (a incorrect mask causes remote IPs to be treated as local).
4. **Router Routing Table:** Check if the default gateway router has an active route (`0.0.0.0/0`) outward to upstream networks.

#### 75. Explain subnetting to a non-technical person. [N | EL]
* **Analogy:** "Think of an IP block like a massive apartment building. If everyone lives in one single open floor, it gets noisy and disorganized. Subnetting is like building walls and doors to split the building into separate, secure private apartments. Each apartment gets its own unit number, keeping internal noise inside and preventing neighbors from interfering with each other."

---

### Subnetting Questions & Calculations

#### 76. How many usable hosts are available in a /24 network? [D | EL]
* **Total Bits:** 32 bits total.
* **Host Bits:** $32 - 24 = 8$ bits.
* **Total IPs:** $2^8 = 256$.
* **Usable Hosts:** $256 - 2 = \mathbf{254}$ usable host IPs.

#### 77. How many usable hosts are available in a /26 network? [D | EL]
* **Host Bits:** $32 - 26 = 6$ bits.
* **Total IPs:** $2^6 = 64$.
* **Usable Hosts:** $64 - 2 = \mathbf{62}$ usable host IPs.

#### 78. What is the subnet mask for /25, /26, /27, /28, and /30? [D | EL]
* **/25:** `255.255.255.128` (Block size: 128)
* **/26:** `255.255.255.192` (Block size: 64)
* **/27:** `255.255.255.224` (Block size: 32)
* **/28:** `255.255.255.240` (Block size: 16)
* **/30:** `255.255.255.252` (Block size: 4)

#### 79. Find the network address, broadcast address, and usable range for 192.168.10.67/26. [S | EL]
* **Subnet Mask:** `/26` = `255.255.255.192` (Block size = $256 - 192 = 64$).
* **Subnet Multiples:** 0, 64, 128, 192...
* **Network Address:** `192.168.10.64` (since 67 falls into the 64 to 127 block).
* **Broadcast Address:** `192.168.10.127`
* **Usable Host Range:** `192.168.10.65` to `192.168.10.126`

#### 80. Are 192.168.1.10/24 and 192.168.2.10/24 in the same subnet? [S | EL]
* **Answer:** **No.**
* **Explanation:** A `/24` prefix uses the first 3 octets for the network portion (`255.255.255.0`). Device A belongs to network `192.168.1.0/24`, while Device B belongs to network `192.168.2.0/24`. They require a Layer 3 router to communicate.

#### 81. Divide 192.168.1.0/24 into four equal subnets. [S | EL]
To get 4 equal subnets, borrow 2 bits ($2^2 = 4$): $24 + 2 = \mathbf{/26}$ (Block size = 64).
* **Subnet 1:** Network `192.168.1.0/26` | Range: `.1 - .62` | Broadcast: `.63`
* **Subnet 2:** Network `192.168.1.64/26` | Range: `.65 - .126` | Broadcast: `.127`
* **Subnet 3:** Network `192.168.1.128/26` | Range: `.129 - .190` | Broadcast: `.191`
* **Subnet 4:** Network `192.168.1.192/26` | Range: `.193 - .254` | Broadcast: `.255`

#### 82. You need 50 hosts in each subnet. Which prefix would you select? [S | EL]
* **Calculation:** We need at least 50 host IPs.
  * $2^5 - 2 = 30$ hosts (Too small)
  * $2^6 - 2 = 62$ hosts (Sufficient)
* **Selected Prefix:** 6 host bits require 26 network bits ($32 - 6 = 26$). Use **`/26`** (`255.255.255.192`).

#### 83. You need point-to-point links between routers. Which subnet size is commonly suitable? [S | EL]
* **Standard Prefix:** **`/30`** or **`/31`** (RFC 3021).
* **Reasoning:** A `/30` provides 4 total IPs ($2^2 = 4$), giving 2 usable host addresses (1 for each router interface), 1 network address, and 1 broadcast address. Modern point-to-point links often use `/31` to save IP space.

#### 84. A customer has IP 192.168.1.130/25 and gateway 192.168.1.1. Is the gateway valid for that host? [S | EL]
* **Answer:** **No, the gateway is invalid.**
* **Explanation:** A `/25` mask divides the network into two subnets of 128 IPs each:
  * Subnet 1: `192.168.1.0` to `192.168.1.127` (Usable: `.1 - .126`)
  * Subnet 2: `192.168.1.128` to `192.168.1.255` (Usable: `.129 - .254`)
* The host `192.168.1.130` sits in **Subnet 2**, while the gateway `192.168.1.1` sits in **Subnet 1**. They are on different subnets and cannot communicate directly at Layer 2.

#### 85. A user’s IP is correct, but the subnet mask is /16 instead of /24. What problems could occur? [S | EL]
* **Improper Local Traffic Handling:** The host assumes its local network spans `x.x.0.0` to `x.x.255.255`. When attempting to reach devices in adjacent subnets (e.g., `192.168.2.x`), it will send direct ARP requests on the local segment instead of forwarding packets to its default gateway.
* **Asymmetric Routing / Blackholing:** Because the host fails to send outbound traffic to the router for those subnets, inter-subnet connectivity breaks completely.
