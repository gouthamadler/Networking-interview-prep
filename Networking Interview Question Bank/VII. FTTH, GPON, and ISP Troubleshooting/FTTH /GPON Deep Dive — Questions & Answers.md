# Section L: FTTH / GPON Deep Dive — Questions & Answers

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **270** | D | ML | 60 sec | What is GPON, and how does it differ from a point-to-point fiber connection? |
| **271** | D | ML | 60 sec | What is the typical splitting ratio in a GPON deployment, and why does it matter? |
| **272** | D | ML | 60 sec | What wavelengths does GPON use for upstream and downstream traffic? |
| **273** | D | ML | 45 sec | What is the video overlay wavelength used for in some GPON deployments? |
| **274** | D | ML | 60 sec | What is an optical power budget, and why is it important? |
| **275** | D | EL | 45 sec | What optical power range is generally considered healthy for an ONT's receive (RX) signal? |
| **276** | D | ML | 45 sec | What is the difference between single-mode and multi-mode fiber, and which is used for FTTH? |
| **277** | D | EL | 45 sec | What is the difference between SC/APC and SC/UPC connectors? |
| **278** | D | ML | 60 sec | What is an OTDR, and what is it used for? |
| **279** | D | ML | 60 sec | What is the ONT ranging/registration process on a GPON network? |
| **280** | D | ML | 60 sec | What is the difference between GPON, EPON, and XGS-PON? |
| **281** | T | EL | 90 sec | An ONT shows a solid red LOS (Loss of Signal) light. What does this mean, and how do you troubleshoot it? |
| **282** | T | EL | 90 sec | An ONT's optical power reads far weaker than expected, but there's no visible fiber damage. What do you check? |
| **283** | T | ML | 90 sec | Multiple customers on the same splitter suddenly lose service at once. What does that suggest? |
| **284** | T | ML | 90 sec | A single customer loses service while neighbors on the same splitter are unaffected. What does that suggest? |
| **285** | D | ML | 45 sec | Why is fiber bend radius important during installation? |
| **286** | D | ML | 45 sec | What is the difference between fusion splicing and using a mechanical/fast connector? |
| **287** | T | ML | 90 sec | An ONT has good optical power but still fails to register with the OLT. What do you check? |
| **288** | N | EL | 60 sec | Explain to a customer why a "small scratch" on a fiber connector can cause a total loss of service. |

---

### Answers

#### 270. What is GPON, and how does it differ from a point-to-point fiber connection? [D | ML]
* **Definition:** Gigabit Passive Optical Network (GPON) is a point-to-multipoint fiber access architecture where a single fiber strand from the OLT is split (using passive, unpowered optical splitters) to serve many customers.
* **Contrast with Point-to-Point:** A point-to-point design would dedicate one full fiber strand per customer all the way back to the central office — far more expensive to deploy at scale. GPON shares one feeder fiber and one OLT port across dozens of customers, drastically reducing fiber and equipment costs.

#### 271. What is the typical splitting ratio in a GPON deployment, and why does it matter? [D | ML]
* **Typical Ratios:** Common splitting ratios are **1:32** or **1:64** (sometimes achieved via cascaded splitters, e.g., 1:4 then 1:8).
* **Why It Matters:** Each split divides the available optical power among more subscribers — a higher split ratio (like 1:64) means less power budget available per customer, requiring higher-quality fiber runs, connectors, and splices to stay within acceptable loss limits.

#### 272. What wavelengths does GPON use for upstream and downstream traffic? [D | ML]
* **Downstream (OLT to ONT):** **1490 nm**
* **Upstream (ONT to OLT):** **1310 nm**
* **Why Different Wavelengths:** Using separate wavelengths for each direction allows both to travel on the same single fiber strand simultaneously without interfering with each other, using a technique called Wavelength Division Multiplexing (WDM).

#### 273. What is the video overlay wavelength used for in some GPON deployments? [D | ML]
* **Wavelength:** **1550 nm**
* **Purpose:** Some ISPs overlay a separate RF video signal (traditional broadcast TV) onto the same fiber using this third wavelength, allowing legacy cable-TV-style video service to be delivered alongside standard GPON data without needing separate infrastructure.

#### 274. What is an optical power budget, and why is it important? [D | ML]
* **Definition:** The total allowable optical signal loss (measured in dB) between the OLT's transmit power and the minimum receive sensitivity the ONT needs to correctly decode the signal.
* **Importance:** Every splitter, connector, splice, and meter of fiber introduces some loss (attenuation). If cumulative loss exceeds the power budget, the ONT won't receive a usable signal — this is why technicians measure and document optical loss during installation and troubleshooting.

#### 275. What optical power range is generally considered healthy for an ONT's receive (RX) signal? [D | EL]
* **Typical Healthy Range:** Approximately **-8 dBm to -27 dBm**, though the exact acceptable range depends on the specific ONT/OLT vendor specifications.
* **Interpretation:** Remember that dBm is logarithmic and expressed as negative values closer to zero mean a stronger signal — a reading of -15 dBm is stronger than -25 dBm, even though -25 is a "smaller" number in absolute terms.

#### 276. What is the difference between single-mode and multi-mode fiber, and which is used for FTTH? [D | ML]
* **Single-Mode Fiber (SMF):** Uses a much narrower core, allowing light to travel in a single, direct path — supports much longer distances (tens of kilometers) with lower signal loss. **This is what FTTH/GPON uses.**
* **Multi-Mode Fiber (MMF):** Uses a wider core allowing multiple light paths (modes), which causes modal dispersion and limits it to shorter distances — typically used for short data-center or campus-backbone runs, not last-mile access.

#### 277. What is the difference between SC/APC and SC/UPC connectors? [D | EL]
* **SC/UPC (Ultra Physical Contact):** The connector ferrule end-face is polished flat, and appears blue-colored on the connector body.
* **SC/APC (Angled Physical Contact):** The ferrule end-face is polished at an 8-degree angle, appears green-colored, and significantly reduces back-reflection — this is the standard connector type used in most GPON/FTTH deployments, since APC connectors are required at the OLT and typically throughout the ODN.
* **Compatibility Warning:** APC and UPC connectors must never be mixed — physically connecting them can damage the ferrule and cause excessive signal loss.

#### 278. What is an OTDR, and what is it used for? [D | ML]
* **Definition:** An Optical Time-Domain Reflectometer (OTDR) is a testing instrument that sends light pulses down a fiber and measures the reflected/backscattered light to characterize the entire fiber path.
* **Uses:** Identifying the exact distance to a fiber break, measuring loss at individual splices and connectors along the route, and verifying overall fiber quality after installation — much more detailed than a simple power meter reading.

#### 279. What is the ONT ranging/registration process on a GPON network? [D | ML]
* **Definition:** Because many ONTs share the same upstream fiber and transmit at different physical distances from the OLT, GPON uses a "ranging" process to measure each ONT's round-trip delay and assign it a precise time slot for upstream transmission, preventing collisions between ONTs sharing the medium.
* **Registration:** Alongside ranging, the OLT authenticates each ONT using its unique Serial Number and/or a password/registration ID before granting it service — this is why a technician must register a new ONT's serial number on the OLT (or via auto-provisioning) before it can come online.

#### 280. What is the difference between GPON, EPON, and XGS-PON? [D | ML]
* **GPON (Gigabit PON):** ITU-T standard, up to 2.5 Gbps downstream / 1.25 Gbps upstream, most widely deployed by telecom carriers worldwide.
* **EPON (Ethernet PON):** IEEE standard, up to 1 Gbps symmetric, uses native Ethernet framing — more common in some Asian markets and certain enterprise deployments.
* **XGS-PON:** A next-generation evolution of GPON offering symmetric 10 Gbps upstream and downstream, increasingly deployed to support higher-bandwidth residential and business services while remaining compatible with existing GPON splitters and fiber infrastructure.

#### 281. An ONT shows a solid red LOS (Loss of Signal) light. What does this mean, and how do you troubleshoot it? [T | EL]
* **Meaning:** The ONT is receiving no usable optical signal at all on its downstream port.
* **Troubleshooting Steps:**
  1. Check that the fiber patch cord is fully and correctly seated into the ONT's optical port.
  2. Inspect the connector end-face for dirt, damage, or scratches, and clean it with a proper fiber-optic cleaning tool if needed.
  3. Test with a known-good optical power meter or a visual fault locator (VFL) to check for light presence at the ONT.
  4. If no light is present, trace back toward the splitter/OLT to isolate whether the break is at the drop cable, the splitter, or further upstream.

#### 282. An ONT's optical power reads far weaker than expected, but there's no visible fiber damage. What do you check? [T | EL]
1. **Connector Cleanliness:** Even microscopic dust or oil on a connector end-face can cause significant, invisible-to-the-eye signal loss — clean all connectors along the path.
2. **Excessive Bend Radius:** Check for tightly coiled or sharply bent fiber, which causes light to leak out of the core (macrobend loss).
3. **Splice Quality:** A poor-quality fusion splice can introduce more loss than expected without any visible external sign.
4. **Connector Mismatch:** Confirm APC and UPC connectors haven't been mixed anywhere along the path.
5. **Splitter Ratio Assumption:** Confirm the actual splitter ratio in use matches what's documented — an unexpectedly higher split ratio than assumed will reduce power more than anticipated.

#### 283. Multiple customers on the same splitter suddenly lose service at once. What does that suggest? [T | ML]
* **Suggestion:** A shared point of failure upstream of the individual drop cables — most likely a cut or damaged **feeder fiber** between the OLT and the splitter itself, a failed splitter, or an issue at the OLT port serving that splitter.
* **Approach:** Since the fault affects everyone downstream of one shared component, troubleshooting should focus upstream of the splitter rather than checking each customer's individual drop cable or ONT.

#### 284. A single customer loses service while neighbors on the same splitter are unaffected. What does that suggest? [T | ML]
* **Suggestion:** The problem is isolated to that customer's specific **drop cable, connector, or ONT**, rather than anything shared upstream (since the splitter and feeder fiber are clearly still functioning for the neighbors).
* **Approach:** Focus troubleshooting on the individual customer's premises — the fiber run from the splitter/tap to their home, the indoor patch cords, and the ONT itself.

#### 285. Why is fiber bend radius important during installation? [D | ML]
* **Explanation:** Optical fiber relies on total internal reflection to keep light traveling down the core. Bending the fiber tighter than its minimum specified bend radius causes light to escape through the cladding (macrobending loss), increasing attenuation and potentially causing intermittent or degraded service — even without any visible physical damage to the cable.

#### 286. What is the difference between fusion splicing and using a mechanical/fast connector? [D | ML]
* **Fusion Splicing:** Uses heat to permanently weld two fiber ends together with precise core alignment — produces very low loss (typically under 0.1 dB) and is the preferred method for permanent splices, but requires a fusion splicer and trained technician.
* **Mechanical/Fast Connector:** Aligns and clamps two fiber ends together (sometimes with an index-matching gel) without fusing them — faster and requires less specialized equipment, but generally introduces higher loss and is less reliable long-term than a fusion splice.

#### 287. An ONT has good optical power but still fails to register with the OLT. What do you check? [T | ML]
1. **Serial Number Provisioning:** Confirm the ONT's Serial Number (SLID/FSAN) has actually been registered/provisioned correctly on the OLT — a perfectly good optical signal won't help if the OLT doesn't recognize the device.
2. **Authentication Credentials:** If password-based authentication is used in addition to serial number, verify the credentials match what's configured on the OLT.
3. **Firmware Compatibility:** Confirm the ONT model/firmware is supported and compatible with that OLT vendor's platform.
4. **OLT Port Status:** Verify the specific OLT PON port serving that splitter is administratively up and not in an error state.

#### 288. Explain to a customer why a "small scratch" on a fiber connector can cause a total loss of service. [N | EL]
* **Analogy:** "Think of the fiber core like a hair-thin beam of laser light trying to line up perfectly with another hair-thin target. Even a scratch or a speck of dust smaller than what you could see with your eyes can scatter or block that beam completely, the same way covering a flashlight lens with a piece of tape — even a small piece — can block the light from reaching where it needs to go."
