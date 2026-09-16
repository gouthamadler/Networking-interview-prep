# Section E: Switching — VLANs, Trunking, STP, and EtherChannel — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **138** | D | EL | 45 sec | What is a VLAN? |
| **139** | D | EL | 60 sec | Why do organizations use VLANs? |
| **140** | D | EL | 45 sec | What is the difference between an access port and a trunk port? |
| **141** | D | EL | 60 sec | What is 802.1Q tagging? |
| **142** | D | EL | 45 sec | What is a native VLAN? |
| **143** | D | ML | 60 sec | What is VLAN hopping, and how is it prevented? |
| **144** | D | EL | 45 sec | What is VLAN 1, and why is it often avoided for user traffic? |
| **145** | D | ML | 60 sec | What is VTP, and what are its modes? |
| **146** | S | EL | 120 sec | Configure VLAN 10 named USERS and assign switch ports Fa0/1–Fa0/10 to it. |
| **147** | S | EL | 120 sec | Configure a trunk port on Fa0/24 that allows VLANs 10, 20, and 30. |
| **148** | T | EL | 90 sec | A switch shows the correct VLAN, but the connected PC cannot communicate. What do you check? |
| **149** | T | ML | 120 sec | A trunk link is up, but one VLAN's traffic never crosses it. What is your correction? |
| **150** | N | EL | 60 sec | Explain the benefit of VLANs to a small-business owner. |
| **151** | D | EL | 60 sec | What problem does Spanning Tree Protocol (STP) solve? |
| **152** | D | ML | 60 sec | How is a root bridge elected in STP? |
| **153** | D | ML | 60 sec | What are the STP port roles? |
| **154** | D | ML | 60 sec | What are the STP port states, and what happens in each? |
| **155** | D | ML | 60 sec | What is the difference between STP and RSTP? |
| **156** | D | ML | 45 sec | What is PortFast, and when should it be used? |
| **157** | D | ML | 45 sec | What is BPDU Guard? |
| **158** | S | ML | 90 sec | What happens if a user plugs both ends of a cable into two switch ports on the same switch without STP running? |
| **159** | T | ML | 120 sec | Users report intermittent network outages and MAC address flapping across the switch. What do you suspect, and how do you confirm it? |
| **160** | D | EL | 60 sec | What is EtherChannel? |
| **161** | D | ML | 60 sec | What is the difference between LACP and PAgP? |
| **162** | D | ML | 60 sec | Why must EtherChannel member ports have matching configurations? |
| **163** | S | ML | 90 sec | What is the benefit of EtherChannel over a single high-speed uplink? |
| **164** | D | EL | 45 sec | What is a duplex mismatch, and what symptoms does it cause? |
| **165** | D | ML | 60 sec | What is storm control, and what does it protect against? |
| **166** | T | EL | 90 sec | How do you verify which VLAN a port belongs to, and its operational status? |
| **167** | T | ML | 90 sec | How do you verify the STP root bridge and port roles on a switch? |
| **168** | N | EL | 60 sec | Explain Spanning Tree Protocol to a non-technical customer using an analogy. |

---

### Answers

#### 138. What is a VLAN? [D | EL]
* **Definition:** A Virtual Local Area Network (VLAN) is a logical grouping of switch ports that behaves as its own independent broadcast domain, regardless of the physical location of the connected devices.
* **Key Point:** Devices in different VLANs cannot communicate with each other without a Layer 3 device (router or Layer 3 switch) to route between them.

#### 139. Why do organizations use VLANs? [D | EL]
* **Segmentation:** Separates traffic by department, function, or security zone (e.g., Sales, Finance, Guest Wi-Fi) without needing separate physical switches.
* **Reduced Broadcast Traffic:** Confines broadcasts to a smaller logical segment, improving performance on large networks.
* **Security:** Isolates sensitive traffic (e.g., a Voice VLAN or a Server VLAN) from general user traffic.
* **Flexibility:** Devices can be grouped logically regardless of their physical wiring closet or floor location.

#### 140. What is the difference between an access port and a trunk port? [D | EL]
* **Access Port:** Belongs to exactly one VLAN and carries **untagged** traffic. Used to connect end devices like PCs, printers, and IP phones.
* **Trunk Port:** Carries traffic for **multiple VLANs** simultaneously between switches (or to a router/firewall), tagging each frame with its VLAN ID so the receiving device knows which VLAN it belongs to.

#### 141. What is 802.1Q tagging? [D | EL]
* **Definition:** The IEEE standard that inserts a 4-byte tag into an Ethernet frame header, identifying which VLAN the frame belongs to as it crosses a trunk link.
* **Contents:** The tag includes the VLAN ID (12 bits, supporting VLANs 1–4094) and a priority field used for QoS.

#### 142. What is a native VLAN? [D | EL]
* **Definition:** The one VLAN on an 802.1Q trunk whose traffic is sent **untagged**, for backward compatibility with older equipment that doesn't understand tagging.
* **Security Note:** Both ends of a trunk must agree on the native VLAN; a mismatch can leak traffic between VLANs and is a common misconfiguration flagged by CDP.

#### 143. What is VLAN hopping, and how is it prevented? [D | ML]
* **Definition:** An attack where a host on one VLAN gains unauthorized access to traffic on another VLAN, typically via **switch spoofing** (tricking a port into trunking mode) or **double tagging** (exploiting the native VLAN).
* **Prevention:**
  * Explicitly set unused/user-facing ports to access mode (never leave them auto-negotiating to trunk).
  * Disable auto-trunking (`switchport nonegotiate`).
  * Change the native VLAN on trunks to an unused VLAN ID rather than the default VLAN 1.

#### 144. What is VLAN 1, and why is it often avoided for user traffic? [D | EL]
* **Definition:** VLAN 1 is the default VLAN on Cisco switches — all ports belong to it out of the box, and it also carries control-plane traffic like CDP, VTP, and STP BPDUs.
* **Why Avoid It:** Because it's the default and often the native VLAN, it's a common target for VLAN hopping attacks. Best practice is to move user and management traffic to dedicated, explicitly configured VLANs.

#### 145. What is VTP, and what are its modes? [D | ML]
* **Definition:** VLAN Trunking Protocol (VTP) is a Cisco protocol that synchronizes VLAN database information (VLAN names and IDs) across multiple switches in the same domain, so VLANs don't need to be manually created on every switch.
* **Modes:**
  * **Server:** Can create, modify, and delete VLANs; changes propagate to the domain.
  * **Client:** Receives and applies VLAN updates but cannot make changes locally.
  * **Transparent:** Does not participate in the VTP domain — forwards VTP messages but keeps its own independent VLAN database.

#### 146. Configure VLAN 10 named USERS and assign switch ports Fa0/1–Fa0/10 to it. [S | EL]
```
Switch(config)# vlan 10
Switch(config-vlan)# name USERS
Switch(config-vlan)# exit
Switch(config)# interface range fastEthernet 0/1 - 10
Switch(config-if-range)# switchport mode access
Switch(config-if-range)# switchport access vlan 10
Switch(config-if-range)# end
```
* **Verification:** `show vlan brief` to confirm VLAN 10 exists and the correct ports are assigned to it.

#### 147. Configure a trunk port on Fa0/24 that allows VLANs 10, 20, and 30. [S | EL]
```
Switch(config)# interface fastEthernet 0/24
Switch(config-if)# switchport trunk encapsulation dot1q
Switch(config-if)# switchport mode trunk
Switch(config-if)# switchport trunk allowed vlan 10,20,30
Switch(config-if)# end
```
* **Verification:** `show interfaces trunk` to confirm the port is trunking and the allowed VLAN list is correct.

#### 148. A switch shows the correct VLAN, but the connected PC cannot communicate. What do you check? [T | EL]
1. **Interface Status:** Confirm the port is `up/up` (`show interfaces Fa0/x status`) and not `err-disabled`.
2. **Port Mode:** Verify the port is in **access** mode, not accidentally left as trunk.
3. **Cable/Physical Layer:** Check the cable and NIC link light.
4. **IP Configuration:** Confirm the PC has a valid IP address in the correct subnet for that VLAN (a wrong VLAN often shows up as a DHCP/APIPA issue).
5. **Duplicate/Conflicting IP:** Rule out an IP conflict on the segment.

#### 149. A trunk link is up, but one VLAN's traffic never crosses it. What is your correction? [T | ML]
* **Likely Cause:** The VLAN is not included in the trunk's allowed VLAN list, or it has been explicitly pruned.
* **Correction:**
```
Switch(config)# interface fastEthernet 0/24
Switch(config-if)# switchport trunk allowed vlan add 40
```
* **Verification:** `show interfaces trunk` to confirm the VLAN now appears in the allowed and active VLAN list on both ends of the trunk.

#### 150. Explain the benefit of VLANs to a small-business owner. [N | EL]
* **Analogy:** "Think of your office building as one big open room. VLANs are like adding walls to create separate rooms — one for your staff, one for guests, one for your point-of-sale system — without needing to run new cabling. Each 'room' keeps its own traffic private and reduces the noise (broadcast traffic) that every other room has to deal with, even though everyone is still plugged into the same switch."

#### 151. What problem does Spanning Tree Protocol (STP) solve? [D | EL]
* **Problem:** When switches are connected with redundant physical links (for fault tolerance), Layer 2 has no TTL field like Layer 3 does — a frame can loop endlessly, multiplying itself at every switch. This causes **broadcast storms**, MAC table instability, and can crash a network within seconds.
* **Solution:** STP logically blocks redundant paths to create a single loop-free active topology, while keeping the blocked links ready to activate automatically if the primary path fails.

#### 152. How is a root bridge elected in STP? [D | ML]
* **Election Criteria:** Every switch advertises a **Bridge ID (BID)**, made up of a Bridge Priority (default 32768) and its MAC address.
* **Process:** The switch with the **lowest BID** becomes the root bridge. If priorities are tied, the switch with the lowest MAC address wins.
* **Practical Note:** Administrators typically lower the priority manually on the desired core switch to control which device becomes root, rather than leaving it to chance.

#### 153. What are the STP port roles? [D | ML]
* **Root Port:** The one port on each non-root switch with the best (lowest-cost) path back to the root bridge.
* **Designated Port:** The port on each network segment responsible for forwarding traffic toward that segment; every segment has exactly one.
* **Non-Designated (Blocking) Port:** Any port that would create a loop — it receives BPDUs but does not forward data traffic.

#### 154. What are the STP port states, and what happens in each? [D | ML]
* **Blocking:** Discards frames, listens to BPDUs only — prevents loops.
* **Listening:** Prepares to forward; still no frame forwarding or MAC learning; participates in the election process.
* **Learning:** Begins populating the MAC address table but still does not forward user data.
* **Forwarding:** Fully operational — sends and receives data frames normally.
* **Disabled:** The port is administratively shut down.

#### 155. What is the difference between STP and RSTP? [D | ML]
* **STP (802.1D):** Convergence after a topology change can take **30–50 seconds**, moving through Blocking → Listening → Learning → Forwarding.
* **RSTP (802.1w):** Converges in a few seconds by introducing new port roles (Alternate, Backup) and states, and by actively negotiating with neighboring switches instead of relying on fixed timers.

#### 156. What is PortFast, and when should it be used? [D | ML]
* **Definition:** A Cisco feature that allows an access port to skip the Listening and Learning STP states and move directly to Forwarding.
* **When to Use:** Only on ports connected to **end devices** (PCs, printers, phones) that will never introduce a switch or loop — never on a port connecting to another switch, as it can create a loop that STP won't catch in time.

#### 157. What is BPDU Guard? [D | ML]
* **Definition:** A security feature typically paired with PortFast that immediately puts a port into **err-disabled** state if it receives a BPDU, since a legitimate end device should never send one.
* **Purpose:** Protects against a user accidentally (or maliciously) connecting an unauthorized switch or hub to an access port, which could otherwise introduce a loop or unwanted root bridge election.

#### 158. What happens if a user plugs both ends of a cable into two switch ports on the same switch without STP running? [S | ML]
* **Result:** A **Layer 2 loop** forms instantly. Broadcast frames are duplicated and forwarded endlessly between the two ports, rapidly consuming all available bandwidth and CPU resources — this is a **broadcast storm**.
* **Symptoms:** The switch becomes unresponsive, the MAC address table flaps continuously as it "learns" the same MAC on both ports, and the entire network segment can go down within seconds.
* **Prevention:** STP should always be enabled by default; storm control and BPDU Guard add further protection.

#### 159. Users report intermittent network outages and MAC address flapping across the switch. What do you suspect, and how do you confirm it? [T | ML]
* **Suspicion:** A Layer 2 loop, likely caused by a misconfigured or missing STP link, an unmanaged switch/hub creating an unintended redundant path, or a failed STP negotiation.
* **Confirmation Steps:**
  1. Run `show spanning-tree` to check for unexpected topology changes or a port stuck in a non-standard state.
  2. Check `show mac address-table` repeatedly — a MAC address rapidly moving between two ports confirms flapping consistent with a loop.
  3. Check interface counters for abnormally high broadcast/multicast traffic (`show interfaces` output).
  4. Physically trace cabling for any unintended redundant connection, especially from unmanaged switches brought in by end users.

#### 160. What is EtherChannel? [D | EL]
* **Definition:** A Cisco technology that bundles multiple physical switch links (typically 2–8) into a single logical link, providing increased bandwidth and redundancy while appearing as one interface to STP (avoiding the need to block redundant links).

#### 161. What is the difference between LACP and PAgP? [D | ML]
* **LACP (Link Aggregation Control Protocol):** An open IEEE standard (802.3ad) for negotiating and forming EtherChannel bundles; works across vendors.
* **PAgP (Port Aggregation Protocol):** A Cisco-proprietary equivalent that performs the same function but only works between Cisco devices.
* **Practical Note:** LACP is generally preferred for interoperability in mixed-vendor environments.

#### 162. Why must EtherChannel member ports have matching configurations? [D | ML]
* **Requirement:** All ports in the bundle must match on speed, duplex, trunk/access mode, allowed VLANs, and native VLAN.
* **Reason:** EtherChannel treats the bundle as a single logical link — inconsistent settings across member ports would create unpredictable forwarding behavior and can cause the bundle to fail to form or partially form.

#### 163. What is the benefit of EtherChannel over a single high-speed uplink? [S | ML]
* **Bandwidth Aggregation:** Combines the throughput of multiple physical links (e.g., four 1 Gbps links act as roughly 4 Gbps of aggregate capacity).
* **Redundancy Without STP Blocking:** Because the bundle appears as one logical interface, STP doesn't need to block any of the member links — all of them can actively pass traffic, unlike separate redundant links where STP would block all but one.
* **Resilience:** If one physical link in the bundle fails, traffic redistributes across the remaining links automatically with no STP recalculation delay.

#### 164. What is a duplex mismatch, and what symptoms does it cause? [D | EL]
* **Definition:** Occurs when one end of a link is set to full-duplex and the other to half-duplex (often due to failed auto-negotiation or manual misconfiguration).
* **Symptoms:** Late collisions, CRC errors, and FCS errors visible in `show interfaces`; the connection often appears "up" but performs poorly — slow transfers, intermittent connectivity, and packet loss under load rather than a total outage.

#### 165. What is storm control, and what does it protect against? [D | ML]
* **Definition:** A switch port feature that monitors incoming broadcast, multicast, and/or unicast traffic and automatically suppresses (or shuts down the port) if traffic exceeds a configured threshold within a given interval.
* **Protection:** Guards against broadcast storms caused by Layer 2 loops, misbehaving NICs, or certain denial-of-service attacks, preventing them from consuming all available switch bandwidth.

#### 166. How do you verify which VLAN a port belongs to, and its operational status? [T | EL]
* **Commands:**
  * `show vlan brief` — lists all VLANs and which access ports are assigned to each.
  * `show interfaces Fa0/x switchport` — shows the specific port's administrative and operational VLAN, mode (access/trunk), and native VLAN.
  * `show interfaces Fa0/x status` — shows link status, duplex, speed, and VLAN in a compact summary.

#### 167. How do you verify the STP root bridge and port roles on a switch? [T | ML]
* **Command:** `show spanning-tree`
* **What to Look For:** The "This bridge is the root" line (or the root's Bridge ID if it isn't), the priority value, and a per-interface breakdown showing each port's role (Root, Designated, Alternate) and state (Forwarding, Blocking).

#### 168. Explain Spanning Tree Protocol to a non-technical customer using an analogy. [N | EL]
* **Analogy:** "Imagine two roads connecting the same two towns, and cars are allowed to enter from either end. Without any traffic rules, cars could end up circling between the towns forever, jamming both roads. Spanning Tree Protocol is like a traffic controller that closes one of the roads to prevent that endless loop, but keeps it ready to reopen instantly as a detour if the main road ever gets blocked."
