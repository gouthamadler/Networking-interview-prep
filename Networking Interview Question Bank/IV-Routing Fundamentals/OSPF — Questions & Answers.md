# Section G: OSPF — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **190** | D | EL | 45 sec | What is OSPF? |
| **191** | D | ML | 60 sec | What is the difference between a link-state and a distance-vector routing protocol? |
| **192** | D | ML | 60 sec | What is an OSPF area, and why does OSPF use areas? |
| **193** | D | ML | 45 sec | What is Area 0, and why is it required? |
| **194** | D | ML | 60 sec | How does OSPF calculate its cost metric? |
| **195** | D | ML | 60 sec | What conditions must match for two routers to become OSPF neighbors? |
| **196** | D | ML | 60 sec | What are the OSPF neighbor states, in order? |
| **197** | D | ML | 60 sec | What is a DR and BDR, and why does OSPF elect them? |
| **198** | D | ML | 60 sec | On which network types does OSPF elect a DR/BDR? |
| **199** | D | ML | 45 sec | What are OSPF Hello and Dead timers? |
| **200** | D | ML | 45 sec | What is a Router ID, and how is it determined? |
| **201** | D | ML | 60 sec | What is the purpose of the OSPF wildcard mask in the network command? |
| **202** | S | ML | 120 sec | Configure OSPF process 1 to advertise network 192.168.10.0/24 in Area 0. |
| **203** | T | ML | 120 sec | Two routers are not forming an OSPF neighbor relationship. What do you check? |
| **204** | T | ML | 90 sec | An OSPF neighbor is stuck in the EXSTART/EXCHANGE state. What is the likely cause? |
| **205** | T | EL | 90 sec | How do you verify OSPF neighbors and the routes OSPF has learned? |
| **206** | D | ML | 60 sec | What is OSPF authentication, and why would you enable it? |
| **207** | D | ML | 45 sec | What is a passive interface in OSPF, and when would you use it? |
| **208** | N | EL | 60 sec | Explain OSPF to a non-technical customer using an analogy. |

---

### Answers

#### 190. What is OSPF? [D | EL]
* **Definition:** Open Shortest Path First (OSPF) is an open-standard, link-state Interior Gateway Protocol (IGP) that dynamically discovers and advertises routes within an autonomous system, calculating the best path using Dijkstra's Shortest Path First (SPF) algorithm.
* **Key Traits:** Fast convergence, scalable through the use of areas, and vendor-independent (unlike EIGRP).

#### 191. What is the difference between a link-state and a distance-vector routing protocol? [D | ML]
* **Link-State (e.g., OSPF):** Each router builds a complete map (topology database) of the entire area by exchanging link-state advertisements with all routers, then independently calculates the best path using SPF. Converges quickly and scales well.
* **Distance-Vector (e.g., RIP):** Each router only knows the distance (metric) and direction (next-hop) to a destination, based on information passed from directly connected neighbors — "routing by rumor." Converges more slowly and is more prone to loops.

#### 192. What is an OSPF area, and why does OSPF use areas? [D | ML]
* **Definition:** A logical grouping of routers and links that share the same detailed topology database (Link-State Database).
* **Purpose:** Dividing a large network into areas limits the scope of SPF recalculations and LSA flooding to that area, significantly reducing CPU load and improving scalability compared to running one flat, massive OSPF domain.

#### 193. What is Area 0, and why is it required? [D | ML]
* **Definition:** Area 0 (the "backbone area") is the mandatory central area in any multi-area OSPF design.
* **Requirement:** Every other non-backbone area must connect directly to Area 0 (or through a virtual link), because OSPF requires all inter-area traffic to pass through the backbone — this prevents routing loops between areas.

#### 194. How does OSPF calculate its cost metric? [D | ML]
* **Formula:** Cost = Reference Bandwidth ÷ Interface Bandwidth (default reference bandwidth is 100 Mbps on Cisco IOS).
* **Example:** A 100 Mbps FastEthernet interface has a default OSPF cost of 1; a 10 Mbps Ethernet interface has a cost of 10.
* **Practical Note:** On modern networks with multi-gigabit links, the default reference bandwidth is often manually increased, since anything faster than 100 Mbps would otherwise calculate to the same cost of 1.

#### 195. What conditions must match for two routers to become OSPF neighbors? [D | ML]
* Same **Area ID** on the connecting interfaces.
* Matching **Hello and Dead timer** intervals.
* Matching **authentication** type and credentials (if configured).
* Matching **subnet/mask** on the shared segment.
* Matching **MTU** size (mismatched MTU can cause neighbors to get stuck during database exchange).
* No duplicate **Router IDs** on the segment.

#### 196. What are the OSPF neighbor states, in order? [D | ML]
1. **Down:** No Hello packets received yet.
2. **Init:** A Hello has been received, but the router hasn't seen its own Router ID in the neighbor's Hello yet.
3. **2-Way:** Bidirectional communication confirmed; DR/BDR election occurs here on multi-access networks.
4. **ExStart:** Routers negotiate who will be the "master" for database exchange.
5. **Exchange:** Routers exchange Database Description (DBD) packets summarizing their link-state databases.
6. **Loading:** Routers request any missing detailed LSAs from each other.
7. **Full:** Routers have fully synchronized link-state databases and are true OSPF neighbors.

#### 197. What is a DR and BDR, and why does OSPF elect them? [D | ML]
* **DR (Designated Router):** The single router responsible for generating LSAs on behalf of the multi-access segment and centralizing database synchronization.
* **BDR (Backup Designated Router):** Stands ready to immediately take over if the DR fails.
* **Purpose:** On a multi-access network (like Ethernet) with many routers, having every router form a full adjacency with every other router would create excessive LSA flooding. Instead, all routers only form full adjacencies with the DR and BDR, drastically reducing overhead.

#### 198. On which network types does OSPF elect a DR/BDR? [D | ML]
* **Elected On:** Broadcast multi-access networks (e.g., Ethernet) and non-broadcast multi-access (NBMA) networks like Frame Relay.
* **Not Elected On:** Point-to-point links (e.g., serial links between two routers), since there are only two routers on the segment and full adjacency between them is not a scaling concern.

#### 199. What are OSPF Hello and Dead timers? [D | ML]
* **Hello Timer:** How often a router sends Hello packets to discover and maintain neighbor relationships — default is **10 seconds** on broadcast networks.
* **Dead Timer:** How long a router waits without receiving a Hello before declaring a neighbor down — default is **40 seconds** (4x the Hello interval).

#### 200. What is a Router ID, and how is it determined? [D | ML]
* **Definition:** A unique 32-bit identifier (formatted like an IP address) that identifies a router within the OSPF domain.
* **Determination Order:**
  1. Manually configured Router ID (`router-id` command) — always takes priority if set.
  2. The highest IP address among the router's configured **loopback interfaces**.
  3. The highest IP address among the router's active **physical interfaces**, if no loopback exists.

#### 201. What is the purpose of the OSPF wildcard mask in the network command? [D | ML]
* **Definition:** OSPF's `network` command uses an inverse (wildcard) mask instead of a standard subnet mask to specify which interfaces should participate in OSPF and which area they belong to.
* **Example:** `network 192.168.10.0 0.0.0.255 area 0` matches any interface with an IP in the `192.168.10.0/24` range — the `0` bits in the wildcard mean "must match," and the `255` bits mean "don't care."

#### 202. Configure OSPF process 1 to advertise network 192.168.10.0/24 in Area 0. [S | ML]
```
Router(config)# router ospf 1
Router(config-router)# network 192.168.10.0 0.0.0.255 area 0
```
* **Verification:** `show ip ospf interface brief` to confirm the interface is participating in Area 0, and `show ip route ospf` to confirm the network is being advertised/learned as expected.

#### 203. Two routers are not forming an OSPF neighbor relationship. What do you check? [T | ML]
1. **Physical/Layer 2 Connectivity:** Confirm the link is up and both interfaces can ping each other.
2. **Area Mismatch:** Verify both interfaces are configured in the same area.
3. **Timer Mismatch:** Confirm Hello and Dead timers match on both sides (`show ip ospf interface`).
4. **Authentication Mismatch:** Verify authentication type and password/key match if configured.
5. **Subnet Mismatch:** Confirm both interfaces are actually on the same IP subnet.
6. **ACLs:** Ensure no access list is blocking OSPF's multicast traffic (224.0.0.5/224.0.0.6) or protocol number 89.

#### 204. An OSPF neighbor is stuck in the EXSTART/EXCHANGE state. What is the likely cause? [T | ML]
* **Most Likely Cause:** An **MTU mismatch** between the two routers' interfaces. If one side sends a Database Description packet larger than the other side's configured MTU, the exchange stalls and the neighbor relationship never progresses past this stage.
* **Verification:** Compare `show interfaces` MTU values on both routers, and adjust with `ip mtu` if needed.

#### 205. How do you verify OSPF neighbors and the routes OSPF has learned? [T | EL]
* **Neighbors:** `show ip ospf neighbor` — displays neighbor Router ID, state (should show `FULL`), and the interface used.
* **Routes:** `show ip route ospf` — displays routes learned via OSPF, marked with the `O` code.
* **Database:** `show ip ospf database` — displays the full link-state database for deeper troubleshooting.

#### 206. What is OSPF authentication, and why would you enable it? [D | ML]
* **Definition:** A security feature that requires OSPF routers to validate a shared credential (plaintext or MD5-hashed) before accepting Hello packets and forming a neighbor relationship.
* **Purpose:** Prevents unauthorized or rogue devices from injecting false routing information into the OSPF domain, which could otherwise redirect or black-hole traffic.

#### 207. What is a passive interface in OSPF, and when would you use it? [D | ML]
* **Definition:** A command (`passive-interface`) that stops an interface from sending or receiving OSPF Hello packets, while still allowing its connected network to be advertised into OSPF.
* **When to Use:** On interfaces facing end-user LANs or server segments where there are no other routers to form a neighbor relationship with — this reduces unnecessary OSPF traffic and closes off a potential attack surface for rogue OSPF neighbors.

#### 208. Explain OSPF to a non-technical customer using an analogy. [N | EL]
* **Analogy:** "Imagine everyone on a road-trip planning committee sharing the exact same detailed map of every road, bridge, and traffic condition, rather than just passing along secondhand directions from person to person. Because everyone has the identical full picture, each person can independently calculate the fastest route from where they are — that's what OSPF does for routers: they all build the same detailed map of the network and each calculate their own best path from it."
