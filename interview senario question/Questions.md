# Networking Interview Prep — Q&A Bank

Built from a full study session covering encapsulation, subnetting, switching/VLANs, routing, DHCP/DNS, NAT, troubleshooting methodology, and wireless/security. Every answer below is written the way you'd actually say it out loud in an interview — not a textbook definition.

---

## Module 1: How Data Moves (Encapsulation)

**Q: A switch forwards a frame based on which address?**
> A switch works at Layer 2, so it forwards based on the destination MAC address. It builds a MAC address table by watching which MAC showed up on which port, and uses that to decide where to send traffic.

**Q: A router forwards packets based on which address?**
> Routers work one layer up, at Layer 3, so they forward based on the destination IP address — they check their routing table and pick the best next hop.

**Q: What does the Transport layer add that IP alone doesn't give you?**
> IP gets the packet to the right device, but it doesn't know which application on that device should get it. That's what port numbers do — port 443 for HTTPS, port 53 for DNS, and so on. TCP/UDP is what tells your OS "this data is for the browser, not the email client."

**Q: What happens to data as it travels down the stack on the sending device?**
> Each layer wraps it with its own header — that's encapsulation. Port info gets added, then IP addresses, then MAC addresses, then it goes out as bits. The receiving device does the reverse and strips each header off as it goes back up.

**Q: Walk me through what happens when you type a website into your browser.**
> First, your browser checks if it already has the site's IP cached; if not, it asks DNS to resolve the domain name into an IP address. Once it has that, it forms an HTTP request. That request gets encapsulated — TCP adds a port and reliability info, IP adds the source and destination addresses, and Ethernet adds MAC addresses, using ARP to find your default gateway's MAC if needed. The frame goes out, gets routed hop by hop based on IP, and reaches the server. The server processes the request and sends a response back through the same layered process in reverse, and your browser de-encapsulates it and renders the page. Worth noting — DHCP isn't part of this specific flow; that already happened earlier when your device joined the network and got its own IP, gateway, and DNS server.

---

## Module 2: Subnetting

**The 5-step method (this is what actually matters — memorize the method, not a table):**
1. Interesting octet = round up (prefix ÷ 8)
2. Bits used in that octet = prefix − 8×(octet number − 1)
3. Mask value = 256 − 2^(8 − bits used)
4. Block size = 2^(8 − bits used)
5. Usable hosts = 2^(32 − prefix) − 2

**Q: For 192.168.50.90/27, what's the network address?**
> Block size for /27 is 32. The blocks go 0, 32, 64, 96 — 90 falls between 64 and 95, so the network address is 192.168.50.64, and broadcast is .95.

**Q: For 172.20.10.200/28, what are the network and broadcast addresses?**
> /28 has a block size of 16. Running through the blocks — 192 to 207 is the one 200 falls into. So the network address is 172.20.10.192 and the broadcast is 172.20.10.207.

**Q: What are the usable hosts and block size for a /29?**
> /29 leaves 3 host bits, so 2 cubed minus 2 gives you 6 usable hosts. Block size is 256 minus 248, which is 8.

**Q: Is 192.168.1.100/25 on the same subnet as 192.168.1.5?**
> Yes. /25 has a block size of 128, so the blocks are 0–127 and 128–255. Both .100 and .5 fall in the 0–127 range, so they're on the same subnet — 192.168.1.0/25.

**Q: What does /0 mean, and where would you actually see it?**
> /0 means zero bits are fixed, so it matches every possible address — that's the definition of a default route, written as 0.0.0.0/0. It shows up in routing tables as "if nothing more specific matches, send it here" — that's what makes a default gateway work.

---

## Module 3: Switching & VLANs

**Q: What's the difference between an access port and a trunk port?**
> An access port carries traffic for one VLAN only, and the frames go out untagged — the device on the other end has no idea VLANs even exist. A trunk port carries multiple VLANs over one physical link, and it uses 802.1Q tagging to mark which VLAN each frame belongs to. That's how one cable between two switches can carry all the VLANs at once without mixing them up.

**Q: A trunk port allows VLANs 10, 20, 30. A frame tagged for VLAN 50 arrives — what happens?**
> It gets dropped. The trunk's allowed VLAN list is basically a whitelist, and VLAN 50 isn't on it, so the switch discards the frame. No error gets sent back, nothing gets logged by default — this is actually a common hidden cause of "traffic isn't reaching the other switch" tickets, where the trunk itself is fine but nobody added the new VLAN to the allowed list.

**Q: Two devices on the same switch are in different VLANs — what do they need to communicate?**
> They need something doing Layer 3 routing between the VLANs — either a router with a connection into each VLAN, or a Layer 3 switch doing inter-VLAN routing. Different VLANs are different broadcast domains, so a switch alone can't bridge them.

**Scenario: A port is configured for VLAN 99, which doesn't exist on the switch. The user can't get an IP or reach anything.**
> Since VLAN 99 doesn't exist, the port can't actually forward any traffic — there's no broadcast domain for it to belong to. So when the laptop sends a DHCP request, it never reaches anywhere valid, and no IP comes back. To fix it, I'd go into the interface and reassign it to a VLAN that actually exists — `switchport access vlan 10`, for example — then verify with `show vlan brief` to confirm the VLAN exists and the port shows as a member, and `show interface [port] switchport` to confirm the port's now correctly in access mode on the right VLAN.

---

## Module 4: Routing

**Q: A router has no matching route and no default route. What happens to the packet?**
> It gets dropped, and the router typically sends back an ICMP "destination unreachable" message. There's no fallback — 0.0.0.0/0 is just notation for "match anything," it's not an actual destination packets can be sent to.

**Q: Routes exist for 10.0.0.0/8, 10.1.1.0/25, and 0.0.0.0/0. A packet arrives for 10.1.1.50 — which route wins?**
> The /25 route wins, because of longest prefix match — the most specific matching route always wins, regardless of what else is in the table or which order things were configured in.

**Q: What's the difference between a static route and a directly connected route?**
> A directly connected route is automatic — if a router has an interface with an IP in a given subnet, it just knows that subnet. A static route is something an admin manually configures, saying "to reach this network, go through this next hop."

---

## Module 5: DHCP & DNS

**Q: A user's PC shows IP 169.254.15.20 — what does that tell you?**
> That's an APIPA address, and it specifically means DHCP failed — the device couldn't reach a DHCP server, so Windows self-assigned that address as a fallback. It's basically a signature; the moment you see 169.254.x.x, you go straight to DHCP as the cause.

**Q: What are some real reasons DHCP might fail?**
> A few common ones — the DHCP server itself could be down or unreachable, the DHCP scope could be exhausted so there are no addresses left to hand out, the client could be on the wrong VLAN so its broadcast never reaches the server, or if the DHCP server's on a different subnet, the router might be missing an IP helper-address to relay the request across. Physical layer issues can cause it too, though those are usually more obvious.

**Q: A user can ping a server's IP directly but can't reach it by name. What's isolated?**
> That isolates it to DNS. If the IP works, the network path and the server are both fine — the problem is specifically that the domain name isn't resolving.

**Q: Why does DHCP's Discover and Request use broadcast instead of unicast?**
> Because the client doesn't have an IP address yet — it can't send a unicast packet without a source address, and it also doesn't know the DHCP server's address to unicast to. Broadcast is the only way to reach "whoever's listening" when you have no network identity at all yet.

---

## Module 6: NAT

**Q: 50 devices share one public IP to reach the internet — what NAT type is this?**
> That's PAT, also called NAT overload. It uses unique source ports to track which internal device each connection belongs to, which is what lets many devices share a single public IP at once.

**Q: You want a home server reachable at a fixed public address — which NAT type?**
> Static NAT — one private IP mapped permanently to one specific public IP, so it's always reachable at the same address from outside.

**Q: Is NAT needed for devices to talk to each other within the same private network?**
> No — NAT is only needed when a private IP needs to reach something outside the network, like the internet. Within a LAN, private IPs talk to each other directly.

---

## Module 7: Troubleshooting Methodology

**The core discipline, repeated across every scenario:** scope first, then recent changes, then symptom specifics, then physical/cheap checks, then move up the OSI layers.

**Scenario: "I can't print, and it worked yesterday."**
> First I'd ask if anyone else is having the same issue — that immediately tells me if it's this one PC or the printer/network itself. Then I'd ask if anything changed — new PC, moved desks, recent updates. Then I'd ask what exactly happens when they try to print — error message, nothing happens, stuck in queue. After that I'd check the physical basics — is the printer powered on, showing ready, link light up. Then I'd move to the network layer — can the PC ping the printer's IP, and has the printer's IP possibly changed since yesterday, since that's a very common cause if it's DHCP-assigned. If ping works but printing still fails, I'd check the print spooler and whether the print queue is pointing at the right IP. Once I find the cause and fix it, I'd have the user actually print a test page to confirm it's really resolved, not just assume it is.

**Scenario: "The internet is slow."**
> First I'd ask if it's slow for everyone or just this one device — that splits it between an ISP/router issue versus something specific to them. Then I'd ask what specifically feels slow — browsing, streaming, video calls, downloads — because "slow" means different things depending on what's happening. Then I'd ask if they're on WiFi or wired, since that changes what I check next. And I'd ask when it started, because a sudden change points to something new happening, while "it's always been like this" points more to a capacity or plan issue.

**Q: What's the single highest-value first question when a user reports they can't access a shared resource?**
> Whether anyone else is experiencing the same thing. That one question immediately tells you if you're dealing with a problem on this specific user's machine, or something bigger on the network or server side — and it saves you from troubleshooting the wrong thing first.

---

## Module 8: Wireless & Security Basics

**Q: What's the difference between WPA2 and WEP, and which should you recommend?**
> WEP's encryption is fundamentally broken — it can be cracked in minutes with freely available tools, even though it technically has encryption. WPA2 uses strong encryption that hasn't been broken the same way. You'd always recommend WPA2, or WPA3 if the hardware supports it — never WEP, and never an open network.

**Q: Which part of the CIA triad does a DDoS attack violate?**
> Availability — because the attack floods the system with traffic so the people who are supposed to be able to reach it, can't. It's not about someone seeing data they shouldn't (confidentiality) or data being changed (integrity) — it's specifically about legitimate access being knocked out.

---

## TCP vs UDP (comes up in almost every interview — know this cold)

**Q: What's the difference between TCP and UDP, and when would you use each?**
> TCP is connection-oriented — it does a three-way handshake before sending any data, then acknowledges every segment it receives and resends anything that gets lost. That makes it reliable, but it adds overhead. UDP skips all of that — no handshake, no acknowledgment — so it's faster and lighter, but there's no guarantee your data arrives, or arrives in order. You'd use TCP for things like web browsing, email, or file transfers, where every piece of data has to arrive correctly. You'd use UDP for things like video calls, live streaming, or gaming, where speed matters more than perfect delivery — a dropped frame is less noticeable than the whole call freezing while it waits to resend.

---

*Compiled from a self-paced study session — subnetting method, troubleshooting discipline, and protocol fundamentals aligned to L1/L2 support, NOC, and ISP network support interviews.*
