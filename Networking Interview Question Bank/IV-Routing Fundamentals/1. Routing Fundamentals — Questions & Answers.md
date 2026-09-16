# Section F: Routing Fundamentals — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **169** | D | EL | 45 sec | What is routing? |
| **170** | D | EL | 60 sec | What is the difference between static and dynamic routing? |
| **171** | D | EL | 60 sec | What information does a routing table contain? |
| **172** | D | ML | 60 sec | What is administrative distance? |
| **173** | D | ML | 45 sec | What is a routing metric? |
| **174** | D | EL | 45 sec | What is a directly connected route? |
| **175** | D | EL | 45 sec | What is a default route? |
| **176** | D | ML | 60 sec | What is the difference between a next-hop IP address and an exit interface in a static route? |
| **177** | D | ML | 60 sec | What is the longest prefix match rule? |
| **178** | D | ML | 60 sec | What is a floating static route? |
| **179** | D | ML | 60 sec | What is recursive route lookup, and why can it be a problem? |
| **180** | S | EL | 120 sec | Configure a static route to reach 192.168.20.0/24 via next-hop 10.0.0.2. |
| **181** | S | EL | 120 sec | Configure a default route pointing to 203.0.113.1. |
| **182** | S | ML | 120 sec | Configure a floating static route as a backup to a primary static route. |
| **183** | T | EL | 90 sec | A router has a route to a destination, but packets are not reaching it. What do you check? |
| **184** | T | ML | 120 sec | Two routes exist to the same destination network with different prefix lengths. Which one is used, and why? |
| **185** | T | ML | 120 sec | A static route disappears from the routing table even though it was configured correctly. What is the likely cause? |
| **186** | S | EL | 90 sec | A packet arrives at a router with no matching route and no default route. What happens to it? |
| **187** | D | EL | 45 sec | What is a routing loop, and what mechanism prevents packets from looping forever? |
| **188** | T | EL | 90 sec | How do you view and interpret a router's routing table? |
| **189** | N | EL | 60 sec | Explain routing to a non-technical customer using an analogy. |

---

### Answers

#### 169. What is routing? [D | EL]
* **Definition:** The process by which a Layer 3 device (router or Layer 3 switch) selects the best path to forward a packet from a source network to a destination network that isn't directly connected.
* **Key Point:** Routing operates using IP addresses, whereas switching (Layer 2) operates using MAC addresses within the same network.

#### 170. What is the difference between static and dynamic routing? [D | EL]
* **Static Routing:** Routes are manually configured by an administrator. Predictable and secure, but does not automatically adapt to topology changes and doesn't scale well in large networks.
* **Dynamic Routing:** Routers use a routing protocol (e.g., OSPF, EIGRP, BGP) to automatically discover networks and adjust to topology changes, at the cost of additional CPU/memory overhead and configuration complexity.

#### 171. What information does a routing table contain? [D | EL]
* **Destination Network:** The network address and prefix length (e.g., `192.168.10.0/24`).
* **Next-Hop IP or Exit Interface:** Where to forward packets destined for that network.
* **Administrative Distance:** Trustworthiness of the route's source.
* **Metric:** The cost value used to select the best path when multiple routes exist via the same protocol.
* **Route Source Code:** A letter code (e.g., `C` for connected, `S` for static, `O` for OSPF) indicating how the route was learned.

#### 172. What is administrative distance? [D | ML]
* **Definition:** A value (0–255) representing how trustworthy a routing information source is. When multiple sources (e.g., static and OSPF) advertise a route to the **same destination**, the router installs the route with the **lowest** administrative distance.
* **Common Defaults:** Directly Connected = 0, Static = 1, EIGRP = 90, OSPF = 110, RIP = 120.

#### 173. What is a routing metric? [D | ML]
* **Definition:** A protocol-specific value used to determine the "cost" of a path, used to select the best route **among multiple paths learned via the same protocol**.
* **Examples:** OSPF uses cost (based on bandwidth), EIGRP uses a composite metric (bandwidth and delay by default), and RIP uses hop count.
* **Key Distinction from Administrative Distance:** AD picks between different protocols; metric picks between paths within the same protocol.

#### 174. What is a directly connected route? [D | EL]
* **Definition:** A network that appears automatically in the routing table simply because the router has an active interface configured with an IP address on that subnet — no routing protocol or static configuration is required.
* **Route Code:** Shown as `C` in `show ip route` output.

#### 175. What is a default route? [D | EL]
* **Definition:** A "route of last resort" (`0.0.0.0/0`) that matches any destination not explicitly found elsewhere in the routing table. Commonly used to point all unmatched traffic toward an ISP or upstream gateway.
* **Route Code:** Shown as `S*` when configured statically.

#### 176. What is the difference between a next-hop IP address and an exit interface in a static route? [D | ML]
* **Next-Hop IP Address:** Specifies the IP of the neighboring router that should receive the packet; the router must perform a recursive lookup to determine which local interface reaches that IP.
* **Exit Interface:** Specifies the router's own local interface to send the packet out of directly; commonly used on point-to-point links since there's no ambiguity about the next device on the wire.
* **Best Practice:** On multi-access networks (like Ethernet), using the next-hop IP is generally preferred to avoid ARP-related issues that can occur with exit-interface-only static routes.

#### 177. What is the longest prefix match rule? [D | ML]
* **Definition:** When a router has multiple routing table entries that could match a destination IP address, it always selects the route with the **most specific (longest) subnet mask/prefix**, regardless of administrative distance or metric.
* **Example:** If a packet is destined for `192.168.1.5` and the table contains both `192.168.1.0/24` and `192.168.0.0/16`, the router chooses the `/24` route because it is more specific.

#### 178. What is a floating static route? [D | ML]
* **Definition:** A static route configured with a higher (worse) administrative distance than the primary route to the same destination, so it stays out of the routing table unless the primary route fails.
* **Use Case:** Provides an automatic backup path — for example, a static route via a backup ISP link that only becomes active if the primary dynamic route (e.g., OSPF) disappears.

#### 179. What is recursive route lookup, and why can it be a problem? [D | ML]
* **Definition:** When a static route is configured with only a next-hop IP address (not an exit interface), the router must perform an additional lookup in the routing table to determine which physical interface actually reaches that next-hop IP before it can forward the packet.
* **Potential Problem:** If the recursive lookup itself resolves through another route that changes or becomes unstable, it adds processing overhead and, in some designs, can create ambiguity or delay in route resolution compared to a route with an explicit exit interface.

#### 180. Configure a static route to reach 192.168.20.0/24 via next-hop 10.0.0.2. [S | EL]
```
Router(config)# ip route 192.168.20.0 255.255.255.0 10.0.0.2
```
* **Verification:** `show ip route static` to confirm the route appears with code `S`.

#### 181. Configure a default route pointing to 203.0.113.1. [S | EL]
```
Router(config)# ip route 0.0.0.0 0.0.0.0 203.0.113.1
```
* **Verification:** `show ip route` — look for the `S*` entry, which marks it as the candidate default route.

#### 182. Configure a floating static route as a backup to a primary static route. [S | ML]
```
Router(config)# ip route 192.168.20.0 255.255.255.0 10.0.0.2
Router(config)# ip route 192.168.20.0 255.255.255.0 10.0.1.2 200
```
* **Explanation:** The second route uses administrative distance **200**, which is higher (worse) than the default static AD of 1, so it only appears in the routing table if the first route becomes unavailable.

#### 183. A router has a route to a destination, but packets are not reaching it. What do you check? [T | EL]
1. **Interface Status:** Confirm the exit interface (or the interface used to reach the next-hop) is `up/up` (`show ip interface brief`).
2. **Next-Hop Reachability:** Ping the next-hop IP directly to confirm Layer 3 reachability to it.
3. **ACLs:** Check for any access control lists that might be filtering the traffic along the path.
4. **Return Path:** Confirm the destination network also has a route back to the source — asymmetric routing is a very common cause of "one-way" connectivity issues.
5. **Traceroute:** Run `traceroute` to identify exactly where along the path the packet is being dropped.

#### 184. Two routes exist to the same destination network with different prefix lengths. Which one is used, and why? [T | ML]
* **Answer:** The route with the **longer (more specific) prefix** is always preferred, regardless of administrative distance or metric — this is the longest prefix match rule, and it takes priority over every other route-selection criterion.

#### 185. A static route disappears from the routing table even though it was configured correctly. What is the likely cause? [T | ML]
* **Likely Cause:** The next-hop IP address (or exit interface) specified in the static route is no longer reachable — most Cisco IOS implementations require the next-hop to be valid and reachable for a static route to remain active in the table.
* **Investigation:** Check the status of the interface toward that next-hop, confirm the next-hop device is up, and verify there isn't a route to the next-hop itself that has failed (in the case of recursive lookups).

#### 186. A packet arrives at a router with no matching route and no default route. What happens to it? [S | EL]
* **Result:** The router drops the packet.
* **Notification:** If ICMP is not filtered, the router typically returns an **ICMP "Destination Unreachable"** message back to the source, informing it that the destination network is unreachable.

#### 187. What is a routing loop, and what mechanism prevents packets from looping forever? [D | EL]
* **Definition:** A routing loop occurs when packets are forwarded in a circular path between two or more routers due to inconsistent or incorrect routing information, never reaching their destination.
* **Prevention Mechanism:** Every IP packet has a **Time to Live (TTL)** field, decremented by 1 at each router hop. When TTL reaches zero, the packet is discarded and an ICMP "Time Exceeded" message is sent back to the source — this is also the exact mechanism `traceroute` uses to map a path.

#### 188. How do you view and interpret a router's routing table? [T | EL]
* **Command:** `show ip route`
* **Interpretation:**
  * The letter code on the left (`C`, `S`, `O`, `D`, etc.) identifies how the route was learned.
  * The network and prefix show the destination.
  * `[AD/Metric]` in brackets shows the administrative distance and the metric for that entry.
  * `via <next-hop>` shows where the router will forward matching traffic.

#### 189. Explain routing to a non-technical customer using an analogy. [N | EL]
* **Analogy:** "Think of routing like a GPS at an intersection. Every router is like a signpost that only needs to know the next turn to take, not the entire route to your destination. Your data 'asks' each signpost along the way, 'Which direction gets me closer to my destination?' — and it hops from signpost to signpost, sometimes through several different networks, until it finally arrives."
