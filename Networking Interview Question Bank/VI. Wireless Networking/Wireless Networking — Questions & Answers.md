# Section K: Wireless Networking — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **251** | D | EL | 45 sec | What is Wi-Fi, and what standard governs it? |
| **252** | D | EL | 45 sec | What is an SSID? |
| **253** | D | EL | 60 sec | What is the difference between the 2.4 GHz and 5 GHz bands? |
| **254** | D | ML | 60 sec | What role does 6 GHz (Wi-Fi 6E) play compared to 2.4 GHz and 5 GHz? |
| **255** | D | ML | 60 sec | What is a wireless channel, and why does channel overlap cause problems? |
| **256** | D | EL | 45 sec | What is the difference between a wireless router and a standalone access point? |
| **257** | D | ML | 60 sec | What is the difference between an autonomous AP and a lightweight AP managed by a WLC? |
| **258** | D | ML | 60 sec | What is the difference between WPA2-Personal and WPA2-Enterprise? |
| **259** | D | ML | 45 sec | Why is WPA3 considered more secure than WPA2? |
| **260** | D | ML | 60 sec | What is 802.1X, and how does it relate to Enterprise Wi-Fi authentication? |
| **261** | D | ML | 60 sec | What is wireless roaming, and what makes it seamless or disruptive? |
| **262** | D | EL | 45 sec | What is RSSI, and what does it tell you about a wireless connection? |
| **263** | D | ML | 60 sec | What common sources cause Wi-Fi interference? |
| **264** | D | ML | 45 sec | What is co-channel interference versus adjacent-channel interference? |
| **265** | S | EL | 90 sec | A customer's Wi-Fi is slow near the router but drops out entirely in a back bedroom. What's your diagnosis approach? |
| **266** | T | EL | 90 sec | A device keeps disconnecting from Wi-Fi and reconnecting every few minutes. What do you check? |
| **267** | T | ML | 90 sec | Wired devices work fine, but all wireless devices experience slow speeds at the same time each evening. What do you suspect? |
| **268** | T | EL | 90 sec | How would you check a device's current wireless signal strength and connected band/channel? |
| **269** | N | EL | 60 sec | Explain the difference between 2.4 GHz and 5 GHz to a customer choosing which band to connect to. |

---

### Answers

#### 251. What is Wi-Fi, and what standard governs it? [D | EL]
* **Definition:** Wi-Fi is the common brand name for wireless local area networking technology, standardized by the IEEE under the **802.11** family of standards.
* **Common Generations:** 802.11n (Wi-Fi 4), 802.11ac (Wi-Fi 5), 802.11ax (Wi-Fi 6/6E), and 802.11be (Wi-Fi 7), each improving speed, capacity, and efficiency over the last.

#### 252. What is an SSID? [D | EL]
* **Definition:** Service Set Identifier (SSID) is the human-readable network name broadcast by a wireless access point, allowing users to identify and select which wireless network to join.
* **Note:** An SSID can be hidden (not broadcast), but this provides only minimal security benefit since it can still be discovered through passive traffic analysis.

#### 253. What is the difference between the 2.4 GHz and 5 GHz bands? [D | EL]
* **2.4 GHz:** Longer range and better wall penetration, but fewer non-overlapping channels (only 3 in most regions) and more susceptible to interference from other consumer devices (microwaves, Bluetooth, cordless phones).
* **5 GHz:** Shorter range and weaker wall penetration, but far more available channels, wider channel widths, and significantly less congestion — resulting in faster real-world throughput at closer range.

#### 254. What role does 6 GHz (Wi-Fi 6E) play compared to 2.4 GHz and 5 GHz? [D | ML]
* **Definition:** The 6 GHz band, introduced with Wi-Fi 6E, opens up a large amount of brand-new spectrum that is completely free of legacy devices and interference from older Wi-Fi generations.
* **Trade-Off:** Even shorter range than 5 GHz due to the higher frequency, but offers the cleanest, least congested spectrum and supports the widest channels for maximum throughput — ideal for high-bandwidth applications in dense environments.

#### 255. What is a wireless channel, and why does channel overlap cause problems? [D | ML]
* **Definition:** A channel is a specific slice of frequency within a wireless band that an access point transmits on.
* **Overlap Problem:** On 2.4 GHz, channels are spaced closely enough that adjacent channels overlap in frequency — using overlapping channels on nearby access points causes interference and collisions, which is why only channels 1, 6, and 11 are considered truly non-overlapping in most regions.

#### 256. What is the difference between a wireless router and a standalone access point? [D | EL]
* **Wireless Router:** A combined device that provides routing (NAT, DHCP, WAN connectivity), a switch, and a wireless access point all in one — typical of home/small-office setups.
* **Standalone Access Point (AP):** Provides wireless connectivity only, relying on a separate router/switch elsewhere in the network for routing, DHCP, and DHCP — common in business environments with multiple APs covering a larger area.

#### 257. What is the difference between an autonomous AP and a lightweight AP managed by a WLC? [D | ML]
* **Autonomous AP:** Operates independently with its own complete configuration (SSIDs, security settings, channel/power settings) — must be configured and managed individually.
* **Lightweight AP:** Relies on a centralized **Wireless LAN Controller (WLC)** to push configuration, manage RF settings, and coordinate roaming across all APs — far more scalable for environments with many access points, since changes are made once on the controller instead of on every AP individually.

#### 258. What is the difference between WPA2-Personal and WPA2-Enterprise? [D | ML]
* **WPA2-Personal (PSK):** Uses a single shared passphrase that all users/devices enter to connect — simple to set up, but the same key must be changed everywhere if compromised, and there's no way to distinguish which individual user connected.
* **WPA2-Enterprise:** Uses **802.1X** with a RADIUS server to authenticate each user or device with individual credentials (username/password or certificate) — provides per-user accountability and doesn't require a shared secret that everyone knows.

#### 259. Why is WPA3 considered more secure than WPA2? [D | ML]
* **Stronger Handshake:** Replaces WPA2's 4-way handshake (vulnerable to offline dictionary attacks, as exploited by the KRACK attack) with **Simultaneous Authentication of Equals (SAE)**, which resists offline password-guessing attempts.
* **Forward Secrecy:** Even if a password is later compromised, previously captured traffic cannot be decrypted retroactively.
* **Enhanced Open:** WPA3 also introduces opportunistic encryption for open (password-less) networks, encrypting traffic even without a shared password.

#### 260. What is 802.1X, and how does it relate to Enterprise Wi-Fi authentication? [D | ML]
* **Definition:** 802.1X is a port-based network access control standard that requires a device to authenticate (typically via a RADIUS server) before being granted network access — used on both wired switch ports and wireless networks.
* **Relation to Wi-Fi:** WPA2/WPA3-Enterprise uses 802.1X as its authentication framework — the wireless client acts as the "supplicant," the access point/controller as the "authenticator," and a backend RADIUS server as the "authentication server."

#### 261. What is wireless roaming, and what makes it seamless or disruptive? [D | ML]
* **Definition:** Roaming is the process of a wireless client moving its association from one access point to another as it physically moves through a coverage area, ideally without interrupting active sessions.
* **Seamless Roaming:** Achieved when APs share the same SSID/security settings and are coordinated by a WLC or mesh system, with fast roaming standards (like 802.11r) minimizing re-authentication delay.
* **Disruptive Roaming:** Occurs when a device holds onto a weak, distant AP too long ("sticky client" behavior) instead of switching to a closer, stronger one, or when roaming standards aren't supported/enabled, causing a full re-authentication delay noticeable to the user (e.g., a dropped call).

#### 262. What is RSSI, and what does it tell you about a wireless connection? [D | EL]
* **Definition:** Received Signal Strength Indicator (RSSI) is a measurement (in dBm, expressed as a negative number) of how strong a wireless signal is at the receiving device.
* **Interpretation:** Values closer to 0 indicate a stronger signal (e.g., -40 dBm is excellent, -70 dBm is weak/marginal, -80 dBm or worse typically means unreliable connectivity).

#### 263. What common sources cause Wi-Fi interference? [D | ML]
* **Other Wireless Networks:** Neighboring Wi-Fi networks on overlapping channels, especially dense apartment buildings.
* **Non-Wi-Fi Devices:** Microwave ovens, cordless phones, and Bluetooth devices (particularly on 2.4 GHz).
* **Physical Obstructions:** Walls, floors, metal objects, and large appliances that attenuate or reflect signals.
* **Co-Channel Congestion:** Too many access points/clients sharing the same channel, increasing contention for airtime.

#### 264. What is co-channel interference versus adjacent-channel interference? [D | ML]
* **Co-Channel Interference:** Occurs when multiple access points use the exact **same** channel — devices must politely take turns transmitting (via CSMA/CA), reducing effective airtime for everyone but not causing actual signal corruption.
* **Adjacent-Channel Interference:** Occurs when access points use channels that are close but not identical, and their signals overlap in frequency — this causes actual signal corruption and retransmissions rather than orderly turn-taking, generally a worse problem.

#### 265. A customer's Wi-Fi is slow near the router but drops out entirely in a back bedroom. What's your diagnosis approach? [S | EL]
1. **Confirm Baseline:** Test speed and signal strength directly next to the router to rule out an ISP-side or WAN issue.
2. **Signal Strength at Distance:** Check the RSSI in the back bedroom — a very weak or absent signal points to a coverage/range problem rather than congestion.
3. **Obstructions:** Ask about walls, floors, or large appliances between the router and that room, which attenuate signal significantly.
4. **Band Consideration:** If the customer is on 5 GHz, suggest testing the 2.4 GHz band, which travels farther through walls, or recommend a mesh system/extender to fill the coverage gap.

#### 266. A device keeps disconnecting from Wi-Fi and reconnecting every few minutes. What do you check? [T | EL]
1. **Signal Strength:** Check RSSI at the device's location — marginal signal near the edge of coverage often causes repeated drop/reconnect cycles.
2. **Channel Congestion:** Check for high channel utilization or interference from neighboring networks.
3. **Driver/Firmware:** Confirm the device's Wi-Fi driver and the access point's firmware are up to date, since bugs in either can cause instability.
4. **Power-Saving Settings:** Check if the device's Wi-Fi power-saving mode is aggressively dropping the connection to conserve battery.
5. **DHCP Lease Issues:** Rule out a DHCP renewal problem causing a brief disconnect that looks like a Wi-Fi drop.

#### 267. Wired devices work fine, but all wireless devices experience slow speeds at the same time each evening. What do you suspect? [T | ML]
* **Suspicion:** Since wired performance is unaffected, the bottleneck is specific to the wireless medium itself — most likely **channel congestion from neighboring Wi-Fi networks** during peak usage hours (everyone home from work/school), or interference from a specific device only used in the evening (e.g., a microwave, a neighbor's new access point).
* **Investigation:** Run a Wi-Fi scan during the affected time window to check channel utilization and count nearby competing networks, and consider switching to a less congested channel or moving critical devices to 5 GHz/6 GHz.

#### 268. How would you check a device's current wireless signal strength and connected band/channel? [T | EL]
* **Windows:** Hold Ctrl and click the Wi-Fi icon in the system tray to view signal details, or run `netsh wlan show interfaces` in Command Prompt for detailed signal, channel, and band information.
* **macOS:** Hold the Option key and click the Wi-Fi icon in the menu bar to reveal RSSI, channel, and PHY mode details.
* **Access Point/Controller Side:** Most enterprise APs and controllers provide a client details page showing connected band, channel, and signal strength for each associated device.

#### 269. Explain the difference between 2.4 GHz and 5 GHz to a customer choosing which band to connect to. [N | EL]
* **Analogy:** "Think of 2.4 GHz like an AM radio station — it travels a long way and gets through walls easily, but the signal can get crowded and a bit noisy. 5 GHz is more like an FM station — clearer and faster up close, but it doesn't travel through walls as well. If you're right next to the router, go with 5 GHz for speed; if you're further away or on another floor, 2.4 GHz will usually stay connected better."
