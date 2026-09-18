# Section J: NAT, PAT, and IP Services — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **231** | D | EL | 45 sec | What is NAT? |
| **232** | D | EL | 60 sec | Why do networks use NAT? |
| **233** | D | ML | 60 sec | What is the difference between static NAT and dynamic NAT? |
| **234** | D | ML | 60 sec | What is PAT (NAT Overload)? |
| **235** | D | ML | 60 sec | What is the difference between PAT and dynamic NAT? |
| **236** | D | ML | 60 sec | Explain the terms inside local, inside global, outside local, and outside global. |
| **237** | D | EL | 45 sec | What is private IP addressing, and why can't it be routed on the public Internet? |
| **238** | D | EL | 45 sec | What are the RFC 1918 private address ranges? |
| **239** | S | ML | 120 sec | Configure static NAT to map inside host 192.168.1.10 to public IP 203.0.113.10. |
| **240** | S | ML | 120 sec | Configure PAT (NAT overload) so an entire LAN shares one public IP address. |
| **241** | T | EL | 90 sec | How do you verify active NAT translations on a router? |
| **242** | T | ML | 120 sec | Internal hosts can browse the Internet, but an inbound connection from outside to an internal server fails. What is the likely cause? |
| **243** | T | ML | 90 sec | A VoIP or VPN application breaks specifically when NAT is involved, even though basic web browsing works. Why might that happen? |
| **244** | D | ML | 60 sec | What is a NAT table, and what does each entry track? |
| **245** | N | EL | 60 sec | Explain NAT to a non-technical customer using an analogy. |
| **246** | D | EL | 45 sec | What is NTP, and why is it important on a network? |
| **247** | D | EL | 45 sec | What is Syslog, and why do engineers rely on it? |
| **248** | D | ML | 60 sec | What is SNMP, and what are its main components? |
| **249** | D | ML | 45 sec | What is the difference between SNMP polling and SNMP traps? |
| **250** | T | EL | 90 sec | Timestamps in log files from two different devices don't match, making it hard to correlate an incident. What is the likely cause and fix? |

---

### Answers

#### 231. What is NAT? [D | EL]
* **Definition:** Network Address Translation (NAT) is a function, usually performed on a router or firewall, that translates private IP addresses used inside a network into a public IP address (or vice versa) for communication across the Internet.
* **Where It Runs:** Typically at the network's edge, between the internal LAN and the ISP-facing WAN interface.

#### 232. Why do networks use NAT? [D | EL]
* **IPv4 Address Conservation:** Allows an entire organization with hundreds or thousands of internal devices to share one (or a small number of) public IP addresses, easing the exhaustion of available IPv4 addresses.
* **Security Through Obscurity:** Hides internal IP addressing from external networks, since outside hosts only ever see the translated public address.
* **Flexibility:** Allows internal addressing schemes to remain private and consistent even if an organization changes ISPs (and therefore its public IP block).

#### 233. What is the difference between static NAT and dynamic NAT? [D | ML]
* **Static NAT:** A permanent, one-to-one mapping between a specific internal private IP and a specific public IP — commonly used for servers that must always be reachable at the same public address.
* **Dynamic NAT:** Maps internal private IPs to an available public IP drawn from a defined pool, on a first-come, first-served basis — still a one-to-one mapping at any given time, but the specific public IP assigned can change between sessions.

#### 234. What is PAT (NAT Overload)? [D | ML]
* **Definition:** Port Address Translation (PAT), also called NAT Overload, allows many internal private IP addresses to share a **single public IP address** simultaneously by additionally translating the source **port number** for each session, keeping every simultaneous connection distinguishable.
* **Scale:** This is the most common form of NAT in home and small-business routers, since it maximizes the use of a single public IP.

#### 235. What is the difference between PAT and dynamic NAT? [D | ML]
* **Dynamic NAT:** Still one-to-one (one internal IP per one public IP at a time) — limited by how many public IPs are in the pool.
* **PAT:** Many-to-one — many internal IPs share a single public IP concurrently, distinguished by unique source port numbers rather than requiring a large pool of public addresses.

#### 236. Explain the terms inside local, inside global, outside local, and outside global. [D | ML]
* **Inside Local:** The private IP address of an internal host as seen inside the local network (e.g., `192.168.1.10`).
* **Inside Global:** The translated public IP address representing that internal host as seen from the outside Internet (e.g., `203.0.113.10`).
* **Outside Global:** The actual public IP address of the external destination host, as it is known on the Internet.
* **Outside Local:** The address of the external host as it appears to devices inside the local network (usually identical to Outside Global unless NAT is also applied on the outside).

#### 237. What is private IP addressing, and why can't it be routed on the public Internet? [D | EL]
* **Definition:** IP address ranges reserved by RFC 1918 for use exclusively within private networks.
* **Reason Not Routable:** Internet backbone routers are configured to discard traffic sourced from or destined to these ranges, since the same private ranges are reused by millions of different private networks worldwide — without NAT, there would be no way to guarantee a private address is globally unique.

#### 238. What are the RFC 1918 private address ranges? [D | EL]
| Class | Range | CIDR |
| :--- | :--- | :--- |
| Class A | 10.0.0.0 – 10.255.255.255 | 10.0.0.0/8 |
| Class B | 172.16.0.0 – 172.31.255.255 | 172.16.0.0/12 |
| Class C | 192.168.0.0 – 192.168.255.255 | 192.168.0.0/16 |

#### 239. Configure static NAT to map inside host 192.168.1.10 to public IP 203.0.113.10. [S | ML]
```
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside
Router(config-if)# exit
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside
Router(config-if)# exit
Router(config)# ip nat inside source static 192.168.1.10 203.0.113.10
```
* **Verification:** `show ip nat translations` to confirm the static mapping is active.

#### 240. Configure PAT (NAT overload) so an entire LAN shares one public IP address. [S | ML]
```
Router(config)# access-list 1 permit 192.168.1.0 0.0.0.255
Router(config)# interface gigabitEthernet 0/0
Router(config-if)# ip nat inside
Router(config-if)# exit
Router(config)# interface gigabitEthernet 0/1
Router(config-if)# ip nat outside
Router(config-if)# exit
Router(config)# ip nat inside source list 1 interface gigabitEthernet 0/1 overload
```
* **Explanation:** The `overload` keyword enables PAT, and using `interface gigabitEthernet 0/1` (rather than a pool) means all inside hosts matching ACL 1 share the outside interface's own IP address.
* **Verification:** `show ip nat translations` — multiple internal IPs will appear translated to the same public IP but with different port numbers.

#### 241. How do you verify active NAT translations on a router? [T | EL]
* **Command:** `show ip nat translations` — lists current active translation entries (inside local/global and outside local/global).
* **Statistics:** `show ip nat statistics` — shows hit counts, active translations, and the configured NAT interfaces, useful for confirming NAT is actually processing traffic as expected.

#### 242. Internal hosts can browse the Internet, but an inbound connection from outside to an internal server fails. What is the likely cause? [T | ML]
* **Likely Cause:** Outbound NAT (PAT) is working correctly for internally initiated sessions, but there is no **static NAT or port-forwarding rule** allowing unsolicited inbound traffic to reach the internal server — by default, NAT/PAT only permits return traffic for connections that were initiated from the inside.
* **Fix:** Configure a static NAT entry (or port forward) mapping the specific public IP/port to the internal server's private IP/port, and ensure any firewall rule also explicitly permits that inbound traffic.

#### 243. A VoIP or VPN application breaks specifically when NAT is involved, even though basic web browsing works. Why might that happen? [T | ML]
* **Explanation:** Some protocols embed IP address or port information directly inside their payload (not just the packet header) — for example, SIP-based VoIP signaling or certain VPN protocols. Standard NAT only rewrites the header, so the embedded payload information becomes inconsistent with the actual translated address, breaking the session.
* **Common Fixes:** Application Layer Gateway (ALG) features designed to rewrite these embedded references, or using NAT-friendly protocol variants (e.g., IPsec NAT Traversal / NAT-T).

#### 244. What is a NAT table, and what does each entry track? [D | ML]
* **Definition:** The dynamic table a NAT-enabled router maintains to track every active translation currently in progress.
* **Entry Contents:** Inside local address/port, inside global address/port, and (for PAT) the outside destination address/port — together these four values uniquely identify each simultaneous session sharing a translated public IP.

#### 245. Explain NAT to a non-technical customer using an analogy. [N | EL]
* **Analogy:** "Think of your office's private IP addresses like internal employee extensions that only work inside the building. NAT is like the receptionist who, whenever someone calls out, temporarily assigns the call the company's single public phone number so the outside world sees one consistent number, then routes any reply back to the correct employee's extension internally."

#### 246. What is NTP, and why is it important on a network? [D | EL]
* **Definition:** Network Time Protocol (NTP) synchronizes the clocks of network devices to a common, accurate time source.
* **Importance:** Accurate, synchronized timestamps are essential for correlating log events across multiple devices during troubleshooting or security investigations, for certificate validation, and for scheduled tasks/backups to run correctly.

#### 247. What is Syslog, and why do engineers rely on it? [D | EL]
* **Definition:** Syslog is a standard protocol/format used by network devices, servers, and applications to send event and error messages to a centralized logging server.
* **Value:** Centralizing logs from many devices in one place makes it far easier to search, correlate, and retain history for troubleshooting and auditing, rather than manually checking logs on each individual device.

#### 248. What is SNMP, and what are its main components? [D | ML]
* **Definition:** Simple Network Management Protocol (SNMP) is used to monitor and manage network devices remotely.
* **Components:**
  * **Manager:** The central monitoring system (e.g., a network monitoring dashboard).
  * **Agent:** Software running on the managed device (router, switch) that responds to queries and can send alerts.
  * **MIB (Management Information Base):** A structured database defining what data can be queried on a given device (e.g., interface status, CPU load).

#### 249. What is the difference between SNMP polling and SNMP traps? [D | ML]
* **Polling:** The SNMP manager actively and periodically queries agents for status/statistics ("pull" model) — useful for regular monitoring and trend graphs.
* **Traps:** The agent proactively sends an unsolicited alert to the manager the moment a significant event occurs (e.g., an interface going down) — a "push" model that enables near-real-time alerting rather than waiting for the next poll cycle.

#### 250. Timestamps in log files from two different devices don't match, making it hard to correlate an incident. What is the likely cause and fix? [T | EL]
* **Likely Cause:** The devices are not synchronized to a common time source — either NTP is not configured on one or both devices, or they're pointed at different/unreliable NTP servers, or their time zone settings differ.
* **Fix:** Configure both devices to synchronize against the same reliable NTP server(s), verify with `show ntp status` / `show clock detail`, and confirm consistent time zone configuration across devices.
