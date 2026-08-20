#  Common Protocols and Services 

# TCP, UDP, and Ports

| # | Tag | Level | Time | Question |
| :--- | :--- | :--- | :--- | :--- |
| **86** | D | EL | 45 sec | What is TCP? |
| **87** | D | EL | 45 sec | What is UDP? |
| **88** | D | EL | 60 sec | Compare TCP and UDP. |
| **89** | D | EL | 60 sec | Explain the TCP three-way handshake. |
| **90** | D | EL | 45 sec | What are SYN, SYN-ACK, and ACK? |
| **91** | D | EL | 45 sec | What is a TCP port? |
| **92** | D | EL | 60 sec | What is the difference between a well-known and ephemeral port? |
| **93** | D | EL | 45 sec | What is a socket? |
| **94** | D | EL | 45 sec | What is retransmission? |
| **95** | S | EL | 90 sec | A TCP application is slow, but ping is normal. What might you investigate? |
| **96** | S | EL | 90 sec | When might UDP be preferable to TCP? |
| **97** | D | EL | 45 sec | What is port 22 used for? |
| **98** | D | EL | 45 sec | What are ports 53, 67/68, 80, 443, and 3389 used for? |
| **99** | T | ML | 90 sec | How would you determine whether a TCP port is reachable? |
| **100** | T | ML | 90 sec | How would you identify a blocked application port using Wireshark? |

---

### Answers

#### 86. What is TCP? [D | EL]
* **Definition:** Transmission Control Protocol is a connection-oriented, reliable Layer 4 protocol that guarantees delivery of data packets across a network.
* **Key Features:** Establishes a session before sending, uses sequence numbers to order packets, performs error checking, and handles retransmission of lost segments.

#### 87. What is UDP? [D | EL]
* **Definition:** User Datagram Protocol is a connectionless, lightweight Layer 4 protocol that provides fast, best-effort data delivery without error recovery or guarantees.
* **Key Features:** No handshake required, no acknowledgments, minimal header overhead (8 bytes), ideal for real-time or streaming workloads.

#### 88. Compare TCP and UDP. [D | EL]
* **Reliability:** TCP guarantees delivery via acknowledgments; UDP offers no delivery guarantees (best-effort).
* **Connection:** TCP is connection-oriented; UDP is connectionless.
* **Speed/Overhead:** TCP has higher header overhead and slower establishment; UDP has low overhead and low latency.
* **Use Cases:** TCP is used for web traffic (HTTP/HTTPS), file transfers (FTP), and email; UDP is used for DNS, VoIP, streaming, and gaming.

#### 89. Explain the TCP three-way handshake. [D | EL]
1. **SYN:** The client sends a synchronization packet containing an initial sequence number to the server.
2. **SYN-ACK:** The server responds with a packet acknowledging the client's sequence number and sends its own synchronization sequence number.
3. **ACK:** The client sends an acknowledgment back to the server. The connection is now established and data transmission can begin.

#### 90. What are SYN, SYN-ACK, and ACK? [D | EL]
* **SYN (Synchronize):** A control flag used to initiate a connection and synchronize sequence numbers.
* **SYN-ACK:** A combined flag response acknowledging receipt of the SYN while also sending the server's own synchronization flag.
* **ACK (Acknowledgment):** A confirmation flag verifying that a segment or handshake step was successfully received.

#### 91. What is a TCP port? [D | EL]
* **Definition:** A 16-bit logical identifier (ranging from 0 to 65,535) used by operating systems to direct incoming network traffic to the correct software application or process on a host.

#### 92. What is the difference between a well-known and ephemeral port? [D | EL]
* **Well-Known Ports (0 – 1023):** Assigned by IANA to standard core network services (e.g., HTTP on 80, SSH on 22).
* **Ephemeral Ports (49152 – 65535):** Temporary dynamic ports assigned automatically by client operating systems to identify outbound client connections.

#### 93. What is a socket? [D | EL]
* **Definition:** The unique combination of an IP address and a port number (e.g., `192.168.1.50:443`) that establishes an endpoint for network communications.

#### 94. What is retransmission? [D | EL]
* **Definition:** The automated process where a TCP sender resends a data segment because it did not receive an acknowledgment (ACK) within a specified timeout window.

#### 95. A TCP application is slow, but ping is normal. What might you investigate? [S | EL]
* **Packet Loss & Retransmissions:** Check for high interface error rates or dropped packets causing TCP to back off and retransmit.
* **Windowing / Flow Control:** Investigate window size bottlenecks or TCP buffer exhaustion.
* **Application/Server Bottlenecks:** Check CPU, memory, or disk I/O constraints on the destination server.
* **DNS Latency:** Check if application slowness is caused by slow name resolution queries.

#### 96. When might UDP be preferable to TCP? [S | EL]
* **Scenario:** When speed and low latency are prioritized over 100% data reliability. Examples include VoIP calls, live video streaming, DNS lookups, and multiplayer video games where dropping an occasional packet is preferable to waiting for a TCP retransmission delay.

#### 97. What is port 22 used for? [D | EL]
* **Definition:** Secure Shell (SSH)—used for secure remote command-line login, administrative access, and secure file transfers (SFTP/SCP).

#### 98. What are ports 53, 67/68, 80, 443, and 3389 used for? [D | EL]
* **Port 53:** DNS (Domain Name System)
* **Port 67 / 68:** DHCP (Dynamic Host Configuration Protocol - Server/Client)
* **Port 80:** HTTP (Hypertext Transfer Protocol - Unencrypted web traffic)
* **Port 443:** HTTPS (HTTP Secure - Encrypted web traffic using TLS)
* **Port 3389:** RDP (Remote Desktop Protocol)

#### 99. How would you determine whether a TCP port is reachable? [T | ML]
* **Command Line Tools:** Use `telnet <ip> <port>` or `nc -zv <ip> <port>` (Netcat) to test port responsiveness.
* **PowerShell:** Use `Test-NetConnection -ComputerName <ip> -Port <port>`.
* **Port Scanners:** Run `nmap -p <port> <ip>` to scan and verify service availability.

#### 100. How would you identify a blocked application port using Wireshark? [T | ML]
* **Filtering:** Apply a display filter like `tcp.port == <port>` or look for the specific IP address.
* **Packet Analysis:** Observe outgoing TCP `SYN` packets being sent repeatedly without a corresponding `SYN-ACK` reply from the destination, or look for incoming `TCP RST` (Reset) packets or ICMP `Destination Unreachable (Port Unreachable)` messages sent by a firewall.
