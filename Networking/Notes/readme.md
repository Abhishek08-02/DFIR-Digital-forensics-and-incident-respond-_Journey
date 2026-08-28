## 📘 Day 1 – OSI Model

Today I studied the **OSI (Open Systems Interconnection) Model**, a 7-layer framework used to understand how data travels in a network.

### 🔑 Key Points
- **7 Layers:** Physical → Data Link → Network → Transport → Session → Presentation → Application  
- Each layer performs a **specific function**  
- OSI is a **reference model**, not a protocol  

### 🛠️ Practical Use
- Helps in **troubleshooting network issues**:
  - Physical → hardware problems  
  - Network → IP/routing issues  
  - Transport → connection errors  

### ⚠️ Security View
- Common attacks:
  - Data Link → ARP spoofing  
  - Network → IP spoofing  
  - Transport → SYN flood  
  - Application → SQL injection, XSS  

### 📌 Summary
- Provides a **structured approach to networking**
- Useful for **debugging and security analysis**

---

### 🚀 Progress
✔ Completed: OSI basics, layers, attacks  
📅 Day 1: **Strong foundation**

➡️ Next: TCP/IP Model


## 📘 Day 2 – TCP/IP Model

Today I studied the TCP/IP (Transmission Control Protocol / Internet Protocol) Model, the practical networking model used in real-world communication over the internet.

### 🔑 Key Points
4 Layers: Network Interface → Internet → Transport → Application
More practical and implementation-focused than OSI
Forms the foundation of the internet

### 🛠️ Practical Use
Used in real-world networking (Internet)
Helps understand:
Internet → IP addressing & routing
Transport → TCP (reliable) vs UDP (fast)
Application → HTTP, DNS, FTP

### ⚠️ Security View
Common attacks:
Network Interface → MAC spoofing, ARP spoofing
Internet → IP spoofing, ICMP attacks
Transport → SYN flood, port scanning
Application → SQL injection, XSS, phishing

### 🔄 OSI vs TCP/IP
OSI has 7 layers, TCP/IP has 4 layers
TCP/IP combines:
OSI Application + Presentation + Session → Application Layer
OSI Physical + Data Link → Network Interface Layer

### 📌 Summary
Simplified and practical model for networking
Backbone of modern internet communication
Essential for networking + cybersecurity

---

### 🚀 Progress

✔ Completed: TCP/IP layers, comparison with OSI, attacks
📅 Day 2: Stronger networking understanding

➡️ Next: Wireshark(First Real Capture)


## 📘 Day 3 – Wireshark First Capture

Today I performed my first real packet capture using **Wireshark** on my 
WiFi interface and analysed live network traffic using DNS and TCP filters.

### 🔑 Key Points
- **Wireshark** is a network protocol analyser used to capture and inspect packets
- **Display Filters** narrow down traffic to specific protocols
- `dns` filter shows all DNS queries and responses
- `tcp` filter shows all TCP connections and their states
- DNS queries reveal every domain your machine communicates with

### 🛠️ Practical (Lab Output)
- Captured live traffic on WiFi interface
- Applied `dns` filter — observed queries to:
  - youtube.com, fonts.googleapis.com, fonts.gstatic.com
  - googleleads.g.doubleclick.net, i.ytimg.com
- Applied `tcp` filter — observed multiple TCP connections to port 443 (HTTPS)
- Identified **SYN packets** (new connections) highlighted in the capture
- Saw **ACK packets** confirming established connections

### 🔍 DNS Observations
- Queries go from my machine → DNS server (192.168.0.1)
- Responses return resolved IP addresses
- Example: `youtube.com` resolved to `192.178.211.136`
- Both **A records** (IPv4) and **HTTPS records** were visible

### ⚠️ Security View
- DNS traffic is **unencrypted by default** — attackers can see every domain queried
- Malware uses DNS to find C2 (Command & Control) servers
- Unusual domains in DNS = first sign of infection
- **Doubleclick.net** entries show ad-tracking activity

### 📌 Summary
- Wireshark captures real-time network traffic
- DNS filter reveals browsing behaviour
- TCP filter shows connection patterns
- Every website visit generates multiple DNS queries + TCP connections

---
### 🚀 Progress
✔ Completed: First live capture, DNS analysis, TCP observation  
📅 Day 3: Hands-on with Wireshark  
➡️ Next: TCP 3-Way Handshake (deep dive)


## 📘 Day 4 – TCP 3-Way Handshake

Today I studied the **TCP 3-Way Handshake** — how two hosts establish 
a reliable connection before any data is exchanged.

### 🔑 Key Points
- TCP = connection-oriented, reliable Layer 4 protocol
- Before data transfer, a connection must be established via handshake
- **3 Steps:** SYN → SYN-ACK → ACK

### 🔧 TCP Segment Structure
| Field | Size | Purpose |
|---|---|---|
| Source Port | 16 bits | Sender's port |
| Destination Port | 16 bits | Receiver's port |
| Sequence Number | 32 bits | Tracks byte order |
| Acknowledgement Number | 32 bits | Next expected byte |
| Flags | 6 bits | SYN, ACK, FIN, RST, PSH, URG |
| Window Size | 16 bits | Flow control |

### 🚩 The 6 TCP Flags
- **SYN** – Synchronise, initiate connection
- **ACK** – Acknowledge, confirms receipt
- **FIN** – Finish, close connection
- **RST** – Reset, abruptly terminate
- **PSH** – Push, send data immediately
- **URG** – Urgent, priority data

### 🤝 Handshake Steps
- **Step 1 – SYN** (Client → Server): Seq=100, SYN=1, ACK=0
- **Step 2 – SYN-ACK** (Server → Client): Seq=300, ACK=101
- **Step 3 – ACK** (Client → Server): ACK=301, SYN=0
- Connection is now **ESTABLISHED** ✅

### 🛠️ Lab Output
- Applied filter `tcp.flags.syn==1` in Wireshark
- Found SYN-ACK packet (Packet 101)
- Source Port: 443, Destination Port: 58512
- Flags: 0x012 (SYN, ACK) confirmed
- Acknowledgement number (raw): 2181824275

### ⚠️ Security View (DFIR)
- Every malware C2 connection starts with a SYN packet
- Port scan = many SYNs with no completions
- SYN flood = DoS attack
- `tcp.flags.syn==1` is one of the most used forensics filters

### 📌 Summary
- TCP handshake = foundation of every network connection
- Recognising SYN/SYN-ACK/ACK in Wireshark is a core analyst skill
- Flags reveal the state and intent of every TCP connection

---
### 🚀 Progress
✔ Completed: TCP structure, flags, handshake, Wireshark lab  
📅 Day 4: Hands-on TCP analysis  
➡️ Next: DNS Deep Dive


## 📘 Day 5 – DNS Deep Dive

Today I studied **DNS (Domain Name System)** — the internet's phone book,
and how to analyse it in Wireshark for forensics investigations.

### 🔑 Key Points
- DNS translates domain names → IP addresses
- Example: `google.com` → `142.250.195.42`
- Every website visit generates multiple DNS queries

### 📋 DNS Record Types
| Record | Full Name | Purpose |
|---|---|---|
| A | Address | Domain → IPv4 address |
| AAAA | IPv6 Address | Domain → IPv6 address |
| MX | Mail Exchange | Where to send emails |
| CNAME | Canonical Name | Alias → another domain |
| NS | Name Server | Which server handles DNS |
| TXT | Text | Verification, SPF records |
| PTR | Pointer | Reverse DNS (IP → domain) |

### 🔄 How DNS Works (9 Steps)
1. Browser checks local cache
2. Asks OS resolver (checks hosts file)
3. Asks Recursive Resolver (router/ISP — 192.168.0.1)
4. Resolver asks Root Name Server → "who handles .com?"
5. Root replies → TLD Name Server (handles .com/.in/.org)
6. TLD replies → Google's Authoritative Name Server
7. Authoritative server replies with actual IP
8. Resolver caches answer and sends back to machine
9. Browser connects to that IP address

### 🔁 Recursive vs Iterative
| Type | Who does the work | Used by |
|---|---|---|
| Recursive | Resolver does all work | Your PC → Resolver |
| Iterative | Each server points to next | Resolver → Root/TLD/Auth |

### 🔍 DNS in Wireshark
| Field | What it means |
|---|---|
| Transaction ID | Matches query to response (e.g. 0x8642) |
| Query Type A | Asking for IPv4 address |
| Query Name | Domain being looked up |
| Response IP | IP address returned |
| TTL | How long to cache the answer |

### 🛠️ Lab Output
- Applied `dns` filter in Wireshark
- Found DNS response for `chromewebstore.googleapis.com`
- Transaction ID: 0x22f5
- Query Type: A, Class IN
- Answer RRs: 4
- Response time: 3.464 milliseconds
- Observed queries to: google.com, gstatic.com,
  safebrowsing.google.com, www.google.com

### ⚠️ Security View (DFIR)
- Every malware infection generates DNS queries to find C2 server
- **DGA** = random-looking domains like `xkq7z3mpa.ru` = malware indicator
- **DNS tunneling** = attackers hide data inside DNS queries to exfiltrate
- High DNS query volume to one domain = beaconing
- `dns` filter reveals everything your machine talks to, even in encrypted traffic

### 📌 Summary
- DNS is the first step in every network connection
- The `dns` filter is the most important first step in any PCAP analysis
- Malicious DNS traffic looks different from normal — DGA, beaconing, tunneling

---
### 🚀 Progress
✔ Completed: DNS records, resolution flow, Wireshark DNS analysis  
📅 Day 5: Deep understanding of DNS internals  
➡️ Next: HTTP vs HTTPS — What the attack surface looks like


## 📘 Day 6 – HTTP vs HTTPS

Today I studied **HTTP and HTTPS** — how web traffic works, how TLS
encrypts it, and how forensics investigators can still extract
information from encrypted traffic using SNI.

### 🔑 Key Points
- HTTP = HyperText Transfer Protocol, Layer 7 (Application Layer)
- Port 80 → HTTP (unencrypted)
- Port 443 → HTTPS (encrypted with TLS)

### 📋 HTTP Request Structure
- `GET /index.html HTTP/1.1`
- `Host: www.example.com`
- `User-Agent: Mozilla/5.0`
- `Accept: text/html`
- `Connection: keep-alive`

### 🔧 HTTP Methods
| Method | Purpose | DFIR Relevance |
|---|---|---|
| GET | Request data | Normal browsing |
| POST | Send data to server | Login, form submit, data exfiltration |
| PUT | Upload/update | File upload attacks |
| DELETE | Delete resource | Destructive actions |

> POST requests are critical in DFIR — malware uses POST to send
stolen data to C2 servers

### 📊 HTTP Status Codes
| Code | Meaning | DFIR Note |
|---|---|---|
| 200 | OK – success | Normal |
| 301/302 | Redirect | Malware redirects |
| 403 | Forbidden | Access attempt |
| 404 | Not Found | Scanning/probing |
| 500 | Server Error | Exploitation attempt |

### 🔒 HTTP vs HTTPS
| Feature | HTTP | HTTPS |
|---|---|---|
| Port | 80 | 443 |
| Encryption | None | TLS encrypted |
| Visibility | Full content visible | Only metadata visible |
| Wireshark filter | `http` | `tls` |
| Can see headers? | Yes — everything | No — encrypted |
| Can see domain? | Yes | Yes — via SNI |

### 🔐 TLS Handshake Sequence
1. Client Hello → client says "I want to connect securely"
2. Server Hello → server agrees, sends certificate
3. Key Exchange → both sides agree on encryption keys
4. Encrypted data flows

### 🔍 SNI — Key Forensics Technique
- SNI (Server Name Indication) is a field in TLS Client Hello
- Reveals destination domain even when traffic is fully encrypted
- Tree structure: TLS Client Hello → Extension: server_name
  → Server Name: www.google.com ← visible!
- Even encrypted malware traffic reveals C2 domain via SNI
- You don't need to decrypt the traffic

### 🛠️ Lab Output
**HTTP capture:**
- Applied `http` filter in Wireshark
- Found Windows Update GET requests to `103.88.220.57`
- URL path: `/c/msdownload/update/others/2026/04/...`
- User-Agent: Windows-Update-Agent/10.0
- Host: download.windowsupdate.com
- Response: HTTP/1.1 200 OK

**TLS/HTTPS capture:**
- Applied `tls` filter
- Found Packet 415: Client Hello (SNI=httpbin.org)
- Found Packet 427: Server Hello (SNI=httpbin.org)
- Destination: 18.214.245.199
- Version: TLS 1.2 (0x0303)

### 🔑 Key Wireshark Filters
| Filter | What it shows |
|---|---|
| `http` | All HTTP traffic |
| `http.request.method=="GET"` | Only GET requests |
| `http.request.method=="POST"` | Only POST requests |
| `tls` | All encrypted HTTPS traffic |
| `tls.handshake.type==1` | TLS Client Hello only |
| `tls.handshake.extensions_server_name` | SNI field visible |

### ⚠️ Security View (DFIR)
- POST requests = possible data exfiltration
- SNI reveals C2 domain even in encrypted traffic
- HTTP credentials visible in plaintext
- TLS version matters — old TLS 1.0/1.1 = security risk

### 📌 Summary
- HTTP is fully visible, HTTPS is encrypted but metadata leaks
- SNI is the most important forensics technique for HTTPS traffic
- Always check POST requests and SNI fields in any investigation

---
### 🚀 Progress
✔ Completed: HTTP/HTTPS structure, TLS handshake, SNI analysis  
📅 Day 6: Deep understanding of web traffic forensics  
➡️ Next: Week 1 Review


## 📘 Day 7 – Week 1 Review

Today I answered all 5 Week 1 review questions **from memory** —
no notes, no looking back. Full self-assessment of Week 1 concepts.

### ✅ Q1 — The 7 OSI Layers
- Layer 1 → Physical: manages physical devices and network hardware
- Layer 2 → Data Link: MAC addressing, frame delivery on same network
- Layer 3 → Network: logical addressing and routing (IP addressing)
- Layer 4 → Transport: end-to-end delivery, segmentation (TCP, UDP)
- Layer 5 → Session: manage sessions (create, delete)
- Layer 6 → Presentation: data formatting (SSL/TLS)
- Layer 7 → Application: what users see (HTTP, HTTPS, DNS)

### ✅ Q2 — TCP 3-Way Handshake
- Step 1 – SYN (Client → Server): SYN=1, ISN=100
  "I want to connect, starting at Seq 100"
- Step 2 – SYN-ACK (Server → Client): SYN=1, ACK=1,
  Seq=300, ACK=101 (client seq+1)
  "Acknowledged, I'll start at Seq 300"
- Step 3 – ACK (Client → Server): ACK=1,
  Seq=101, ACK=301 (server seq+1)
  "Got it. Connection Established"
- After this, data transfer begins

### ✅ Q3 — DNS A Record
- A DNS A record maps a domain name → IPv4 address
- Returns a 32-bit IPv4 address
- Most fundamental DNS record type used for hostname resolution
- Example: `nslookup example.com` → returns `93.184.216.34`

### ✅ Q4 — HTTP vs HTTPS in Wireshark
| Feature | HTTP | HTTPS |
|---|---|---|
| Port | 80 | 443 |
| Wireshark filter | `http` | `tls` or `ssl` |
| Payload visible | Yes — fully | No — encrypted |
| What you can see | Full request/response, headers, cookies, POST body, URLs | IP addresses, ports, TLS handshake, certificate info, SNI |
| Protocol shown | HTTP | TLSv1.2 / TLSv1.3 |

### ✅ Q5 — SNI
- SNI (Server Name Indication) is a TLS extension
- Sent in the Client Hello packet
- Tells the server which hostname the client is trying to reach
- Sent **before any encryption begins**
- This means investigators can see the destination domain
  even in fully encrypted HTTPS traffic — without decrypting it

### 📌 Summary
- All 5 questions answered correctly from memory
- Week 1 foundation is solid — OSI, TCP/IP, Wireshark,
  DNS, HTTP/HTTPS all understood

---
### 🚀 Progress
✔ Week 1 Complete — OSI, TCP/IP, Wireshark, DNS, HTTP/HTTPS  
📅 Day 7: Full Week 1 self-assessment passed  
➡️ Next: Week 2 — Subnetting, Protocols, First Malware PCAP


## 📘 Day 8 – Subnetting + IP Addressing

Today I studied **IP addressing and subnetting** — how networks are
divided, how to calculate host ranges, and why it matters for DFIR.

### 🔑 Key Points
- IP address = 32-bit number, written in dotted decimal (4 octets)
- Every IP has two parts: Network portion + Host portion
- Subnet mask tells you where the line is drawn
- CIDR = IP address + prefix length (e.g. 192.168.1.0/24)

### 📋 Subnet Mask Table
| CIDR | Subnet Mask | Network bits | Host bits |
|---|---|---|---|
| /8 | 255.0.0.0 | 8 | 24 |
| /16 | 255.255.0.0 | 16 | 16 |
| /24 | 255.255.255.0 | 24 | 8 |
| /32 | 255.255.255.255 | 32 | 0 |

### 🔧 Key Formulas
1. Total Hosts = 2^(host bits)
2. Usable Hosts = 2^(host bits) - 2 (network & broadcast)
3. Network Address = First IP in range (all host bits = 0)
4. Broadcast Address = Last IP in range (all host bits = 1)
5. First Usable Host = Network Address + 1
6. Last Usable Host = Broadcast Address - 1

### 🧮 Lab Problems Solved

**Problem 1 — 192.168.1.0/24**
- Host bits: 32-24 = 8
- Total hosts: 2^8 = 256
- Usable hosts: 256-2 = 254
- Network address: 192.168.1.0
- Broadcast: 192.168.1.255
- First usable: 192.168.1.1
- Last usable: 192.168.1.254
- Subnet mask: 255.255.255.0

**Problem 2 — 10.0.0.0/8**
- Host bits: 32-8 = 24
- Total hosts: 2^24 = 16,777,216
- Usable hosts: 16,777,214
- Network address: 10.0.0.0
- Broadcast: 10.255.255.255
- Subnet mask: 255.0.0.0

**Problem 3 — 172.16.5.43/20**
- Host bits: 32-20 = 12
- Total hosts: 2^12 = 4096
- Usable hosts: 4094
- Network address: 172.16.0.0
- Broadcast: 172.16.15.255
- First usable: 172.16.0.1
- Last usable: 172.16.15.254
- Subnet mask: 255.255.240.0

### 🌐 Private IP Ranges (RFC 1918)
| Range | CIDR | Used for |
|---|---|---|
| 10.0.0.0 – 10.255.255.255 | /8 | Large networks |
| 172.16.0.0 – 172.31.255.255 | /12 | Medium networks |
| 192.168.0.0 – 192.168.255.255 | /16 | Home/small networks |

> Your home WiFi is 192.168.x.x — that's why you see it in Wireshark!

### ⚠️ Why Subnetting Matters for DFIR
- Every IP in a PCAP belongs to a network — subnetting tells you which
- 192.168.x.x = private/internal traffic
- 8.8.8.8, 142.250.x.x = public/external traffic
- Knowing subnets helps identify **lateral movement** —
  attacker moving between internal subnets
- RFC 1918 private ranges should never appear as
  destination IPs on internet — if they do, something is wrong

### 📌 Summary
- Subnetting is the foundation of network analysis
- Every IP investigation starts with identifying internal vs external
- Subnet math: host bits = 32 - prefix, usable = 2^n - 2

---
### 🚀 Progress
✔ Completed: IP addressing, CIDR, subnet math, 3 lab problems  
📅 Day 8: Subnetting fully understood  
➡️ Next: UDP + ICMP + ARP — Protocols attackers abuse


## 📘 Day 9 – UDP + ICMP + ARP

Today I studied three protocols that attackers frequently abuse —
UDP, ICMP, and ARP — and analysed all three live in Wireshark.

### 🔑 UDP — User Datagram Protocol
- Connectionless, unreliable transport layer protocol (Layer 4)
- No handshake — data sent directly
- 8-byte header only: Source Port, Destination Port, Length, Checksum
- No sequence numbers, no ACK, no flow control

### 🔄 TCP vs UDP
| Feature | TCP | UDP |
|---|---|---|
| Connection | 3-way handshake | No handshake |
| Reliability | Guaranteed delivery | No guarantee |
| Speed | Slower | Faster |
| Order | In order | No order |
| Use case | Web, email, file transfer | Video, DNS, VoIP, gaming |

### ⚠️ UDP in DFIR
- DNS uses UDP (port 53) — most DNS queries are UDP
- DHCP uses UDP (ports 67/68)
- Attackers use UDP for UDP flood attacks (DoS)
- Malware can use UDP to avoid TCP connection tracking
- Filter: `udp`

### 🔑 ICMP — Internet Control Message Protocol
- Layer 3 protocol used for diagnostics and error reporting
- NOT used to transfer data — only control messages

### 📋 Key ICMP Types
| Type | Name | Purpose |
|---|---|---|
| 0 | Echo Reply | Response to ping |
| 3 | Destination Unreachable | Host/port not reachable |
| 8 | Echo Request | The ping itself |
| 11 | Time Exceeded | TTL expired (traceroute) |

### ⚠️ ICMP in DFIR
- Ping sweep — attacker pings many IPs to find live hosts
- ICMP tunneling — data hidden inside ICMP packets for exfiltration
- TTL exceeded — used by traceroute, shows network path
- High ICMP volume = possible reconnaissance
- Filter: `icmp`

### 🔑 ARP — Address Resolution Protocol
- Layer 2 (Data Link) — translates IP addresses → MAC addresses
- IP = logical address (routing), MAC = physical address (local delivery)
- Before sending data, PC must know MAC address of destination
- ARP Cache stores IP→MAC mappings to avoid repeated requests

### 📋 ARP Request vs Reply
| Message | Direction | Purpose |
|---|---|---|
| ARP Request | Broadcast (to everyone) | Who has this IP? |
| ARP Reply | Unicast (to requester) | I have that IP, here's my MAC |

### ⚠️ ARP in DFIR
- ARP Poisoning/Spoofing — attacker sends fake ARP replies
- Causes traffic meant for router to go to attacker → MITM attack
- Signs: same MAC claiming multiple IPs, high ARP rate
- Gratuitous ARP — device announces own IP→MAC unprompted (suspicious!)
- Filter: `arp`

### 🛠️ Lab Output
**UDP:** Applied `udp` filter — found UDP packets including DNS
queries (port 53) and QUIC protocol traffic to 142.250.207.142

**ICMP:** Ran `ping google.com` in terminal
- Packets Sent=4, Received=4, Lost=0 (0% loss)
- TTL=118, Average time=6ms

**ARP:** Applied `arp` filter — found:
- Packet 150: "Who has 192.168.0.142? Tell 192.168.0.1"
- Packet 151: "192.168.0.142 is at [MAC address]"
- Opcode: reply (2), Protocol type: IPv4

### 📌 Summary
- TCP = connection-oriented
- UDP = connectionless
- ICMP = diagnostic
- ARP = local resolution

---
### 🚀 Progress
✔ Completed: UDP, ICMP, ARP — theory + Wireshark lab  
📅 Day 9: Three attacker-abused protocols understood  
➡️ Next: SMTP + FTP + SSH — Email and File Transfer Protocols


## 📘 Day 10 – SMTP + FTP + SSH

Today I studied three application layer protocols used for email and
file transfer, and analysed a real email PCAP from Wireshark samples.

### 🔑 SMTP — Simple Mail Transfer Protocol
- Used to send emails from client to server and between mail servers
- Port 25 → SMTP (server to server)
- Port 587 → SMTP (client to server, modern)
- Port 465 → SMTP over SSL

### 📧 SMTP Conversation
- Client → `EHLO mail.example.com`
- Server → `250 Hello`
- Client → `MAIL FROM: <sender@example.com>`
- Server → `250 OK`
- Client → `RCPT TO: <receiver@example.com>`
- Server → `250 OK`
- Client → `DATA`
- Server → `354 Start Input`
- Client → Subject, From, [email body]
- Server → `250 OK Message accepted`
- Client → `QUIT` / Server → `221 Bye`

### ⚠️ SMTP in DFIR
- Phishing emails travel over SMTP — full email visible in plaintext
- MAIL FROM can be spoofed — attacker puts any address here
- RCPT TO shows who the email was actually delivered to
- Email headers reveal the true origin IP of the sender
- Filter: `smtp`

### 🔑 FTP — File Transfer Protocol
- Used to transfer files between client and server
- Port 21 → FTP Control (commands)
- Port 20 → FTP Data (actual file transfer)
- Completely unencrypted — credentials and files visible in plaintext!

### 📋 FTP Commands
| Command | Purpose |
|---|---|
| USER | Send username |
| PASS | Send password — visible in plaintext! |
| LIST | List files |
| RETR | Download a file |
| STOR | Upload a file |
| QUIT | Disconnect |

### ⚠️ FTP in DFIR
- FTP credentials fully visible in Wireshark — filter: `ftp`
- File contents also visible — filter: `ftp-data`
- Attackers use FTP to exfiltrate stolen data
- Look for STOR commands — files being uploaded OUT
- Look for RETR commands — files being downloaded

### 🔑 SSH — Secure Shell
- Used for secure remote login and encrypted file transfer
- Port 22 — everything is encrypted
- Cannot see commands or data in Wireshark
- Replaces Telnet (port 23) which was unencrypted

### 🔍 SSH in Wireshark
- Can see: connection made (IP + port 22), packet sizes, timing
- Cannot see: actual commands or data
- Filter: `ssh` or `tcp.port==22`

### ⚠️ SSH in DFIR
- SSH to unusual IPs = possible backdoor
- SSH on non-standard ports (not 22) = suspicious
- Large SSH data transfers = possible exfiltration
- Attackers use SSH tunneling to bypass firewalls
- Even though encrypted, connection metadata is valuable

### 📋 Protocol Comparison Table
| Protocol | Port | Encryption | Purpose | DFIR Risk |
|---|---|---|---|---|
| SMTP | 25/587 | None (usually) | Send email | Phishing, spoofing |
| FTP | 21/20 | None | File transfer | Credentials visible |
| SSH | 22 | Full | Secure remote | Tunneling, backdoor |
| Telnet | 23 | None | Remote login | Everything visible |
| IMAP | 143 | None | Receive email | Email theft |
| POP3 | 110 | None | Receive email | Email theft |

### 🛠️ Lab Output
- Downloaded smtp.pcap from wiki.wireshark.org/SampleCaptures
- Applied `smtp` filter in Wireshark
- Followed TCP Stream — saw complete email conversation:
  - EHLO greeting to xc90.websitewelcome.com
  - AUTH LOGIN — authentication
  - 235 Authentication succeeded
  - MAIL FROM / RCPT TO visible
  - DATA section with From, To, Subject, Message body
  - Full email content readable in plaintext

### 📌 Summary
- SMTP + FTP = unencrypted — full content visible in Wireshark
- SSH = encrypted — only metadata visible
- FTP passwords are the easiest credentials to steal from a PCAP
- SMTP MAIL FROM can be spoofed — always check email headers

---
### 🚀 Progress
✔ Completed: SMTP, FTP, SSH — theory + real PCAP analysis  
📅 Day 10: Email and file transfer protocols understood  
➡️ Next: First Malware PCAP Analysis


## 📘 Day 11 – First Malware PCAP Analysis

Today I performed my first real malware PCAP analysis using the
6-step analyst workflow on a real exercise from
malware-traffic-analysis.net.

### 🔑 What is Malware Traffic Analysis?
When malware infects a machine, it generates network traffic.
As a DFIR analyst, your job is to read that traffic and
reconstruct what happened.

### 🔄 The Analyst Workflow — Always Follow This Order
1. Protocol Hierarchy → What protocols exist?
2. Statistics → Conversations → Who is talking to whom?
3. DNS Filter → What domains did it contact?
4. HTTP Filter → What URLs did it request?
5. Export HTTP Objects → Were any files downloaded?
6. Follow TCP Stream → Read the full conversation

### 📋 DNS Indicators
| Pattern | What it means |
|---|---|
| Random-looking domain | DGA — Domain Generation Algorithm |
| Same domain queried repeatedly | Beaconing |
| Long subdomain strings | DNS tunneling |
| Domains ending in .ru, .xyz, .tk | Often malicious |

### 📋 HTTP Indicators
| Pattern | What it means |
|---|---|
| POST to strange URL | Data exfiltration |
| GET of .exe or .dll | Malware download |
| Unusual User-Agent | Malware disguising itself |
| HTTP to IP (no domain) | Direct C2 connection |

### 📋 TCP Indicators
| Pattern | What it means |
|---|---|
| Connection to port 4444 | Metasploit default |
| Connection to port 1337 | Common malware port |
| Many SYN with no response | Port scanning |
| Regular timed connections | Beaconing |

### 🔧 Key Wireshark Techniques
1. Protocol Hierarchy: `Statistics → Protocol Hierarchy`
   Shows every protocol as percentage — unexpected = red flag
2. Conversations: `Statistics → Conversations → TCP tab`
   Suspicious IP with lots of traffic = likely C2
3. Export HTTP Objects: `File → Export Objects → HTTP`
   Extracts executables, scripts, documents
4. Follow TCP Stream: `Right click → Follow → TCP Stream`
   Full conversation in human-readable form
5. Check User-Agent: filter `http.user_agent`
   Malware often uses fake/unusual strings

### 🦠 Common Malware Families
| Malware | Traffic Pattern |
|---|---|
| Emotet | HTTP POST to compromised WordPress sites |
| Trickbot | HTTPS to many IPs, certificate anomalies |
| Cobalt Strike | Beaconing every 60 seconds, HTTPS |
| Dridex | HTTP GET of encrypted payloads |
| Qakbot | HTTPS + DNS with DGA domains |

### 🛠️ Lab Output — Real PCAP Analysis
Applied dns filter and found suspicious domains:

**Finding 1: `modandcrackapk.com`**
- Evidence: DNS filter, A record query
- Resolved to: 193.42.38.139
- Meaning: Suspicious domain — cracked APK distribution site,
  likely malware delivery

**Finding 2: `wpad.nemotoads.health`**
- Evidence: DNS filter, query returned "No such name"
- Meaning: Malware beacon attempting to reach C2 —
  domain already taken down (classic sign of active malware)

### 🔑 Key Filters for Malware Analysis
- `dns` — suspicious domains
- `http` — suspicious URLs
- `tcp.port==4444` — Metasploit
- `http.user_agent` — fake user agents

### 📌 Summary
- Always follow the 6-step workflow before reading write-ups
- DNS filter is the first and most important step
- Suspicious domains + "No such name" = malware beacon
- Real malware traffic looks different from normal traffic

---
### 🚀 Progress
✔ Completed: Malware analysis workflow + real PCAP investigation  
📅 Day 11: First real malware traffic identified  
➡️ Next: Routing Protocols + Network Layer Deep Dive


## 📘 Day 12 – Routing Protocols + Network Layer Deep Dive

Today I studied the IP datagram structure, TTL, routing, IP
fragmentation, and how traceroute works — then verified everything
live in Wireshark and terminal.

### 🔑 IP Datagram Structure
| Field | Size | Purpose |
|---|---|---|
| Version | 4 bits | IPv4 or IPv6 |
| Header Length | 4 bits | Size of header |
| TTL | 8 bits | Hops remaining |
| Protocol | 8 bits | TCP=6, UDP=17, ICMP=1 |
| Source IP | 32 bits | Where packet came from |
| Destination IP | 32 bits | Where packet is going |
| Checksum | 16 bits | Error detection |
| Flags | 3 bits | Fragmentation control |
| Fragment Offset | 13 bits | Position in fragment |

### ⏱️ TTL — Time To Live
- Most important IP field for DFIR
- Every router that forwards a packet decreases TTL by 1
- When TTL reaches 0 → router sends ICMP TTL Exceeded back
- This is exactly how traceroute works!

### 📋 Common TTL Starting Values
| OS | Starting TTL |
|---|---|
| Windows | 128 |
| Linux | 64 |
| Cisco/Network | 255 |

### ⚠️ DFIR Use of TTL
- TTL tells us how far away the source is
- Unusually low TTL = packet has traveled many hops
- TTL=1 packet flood = TTL Attack

### 🔄 How Routing Works
- Router's job: look at destination IP, find best path, forward
- Your PC → Home Router (TTL=63) → ISP Router (TTL=62)
  → Backbone (TTL=61) → Google (TTL=60)
- Every router has a routing table
- 0.0.0.0/0 = default route ("if you don't know, send it here")

### 🔀 IP Fragmentation
- When packet > MTU → fragmented into smaller pieces
- MTU (Maximum Transmission Unit) = max packet size a link can carry
- Ethernet MTU = 1500 bytes
- DFIR: attackers use fragmentation to bypass firewalls
- Filter: `ip.frag_offset > 0`

### 🌐 IPv4 vs IPv6
| Feature | IPv4 | IPv6 |
|---|---|---|
| Address size | 32 bits | 128 bits |
| Format | 192.168.1.1 | 2001:db8::1 |
| Total addresses | ~4 billion | 340 undecillion |
| Header size | 20 bytes | 40 bytes |
| Fragmentation | Routers can fragment | Only source fragments |

### 📋 Protocol Numbers
| Number | Protocol |
|---|---|
| 1 | ICMP |
| 6 | TCP |
| 17 | UDP |
| 47 | GRE (tunneling) |
| 50 | ESP (IPSec) |

### 🔍 How Traceroute Works — TTL Trick
- Send TTL=1 → first router drops it, sends ICMP back
- Send TTL=2 → second router drops it, sends ICMP back
- Send TTL=3 → third router drops it, sends ICMP back
- Each ICMP TTL-Exceeded reply reveals one router's IP!

### 🛠️ Lab Output
**Wireshark ip filter:**
- Found IP packet: Version=4, TTL=128 (Windows!), Protocol=TCP(6)
- Flags: Don't fragment, Fragment Offset=0

**tracert google.com:**
- Completed in 8 hops
- Hop 7: 255ms (high latency — international link)
- Trace complete

**icmp filter during traceroute:**
- Echo (ping) requests with TTL=4,5,6,7
- Time-to-live exceeded messages visible in real time

### 🔑 Key DFIR Filters
- `ip` → All IP traffic
- `ip.ttl < 10` → Unusual low TTL
- `ip.frag_offset > 0` → Fragmented packets
- `icmp.type == 11` → TTL exceeded messages

### 📌 Summary
- TTL is one of the most important fields for DFIR
- Traceroute = clever TTL trick to map network path
- Fragmentation can be used to bypass firewalls
- Protocol number in IP header tells you what's inside

---
### 🚀 Progress
✔ Completed: IP structure, TTL, routing, fragmentation, traceroute  
📅 Day 12: Full network layer understanding  
➡️ Next: NetworkMiner + Traffic Statistics in Wireshark


## 📘 Day 13 – NetworkMiner + Traffic Statistics in Wireshark

Today I installed NetworkMiner and used it alongside Wireshark
Statistics to perform rapid triage on a malware PCAP — the way
real analysts work.

### 🔑 What is NetworkMiner?
- Passive network forensics tool
- Reads a PCAP and automatically extracts everything useful
- Used for rapid triage — get the big picture fast
- No filters needed — everything extracted automatically

### 🔄 NetworkMiner vs Wireshark
| Feature | Wireshark | NetworkMiner |
|---|---|---|
| Primary use | Deep packet inspection | Rapid triage and extraction |
| Learning curve | Steep | Easy |
| Shows hosts | Manual (Statistics menu) | Automatic (Hosts tab) |
| Extracts files | Manual (Export Objects) | Automatic (Files tab) |
| Extracts credentials | Manual (Follow stream) | Automatic (Credentials tab) |
| Best for | Detailed analysis | First 5 minutes of investigation |
| Speed | Slow (manual) | Fast (automatic) |

### 📋 NetworkMiner Tabs
| Tab | What it shows | DFIR Use |
|---|---|---|
| Hosts | All IPs, MAC addresses, OS fingerprint | Who was on the network? |
| Files | Every file transferred | Malware dropped? |
| Credentials | Usernames and passwords | Were creds stolen? |
| DNS | All DNS queries and responses | C2 domains? |
| Images | Images transferred | What was exfiltrated? |
| Parameters | HTTP POST parameters | Form data, stolen info |

### 📊 Wireshark Statistics Menu
**1. Protocol Hierarchy** (`Statistics → Protocol Hierarchy`)
- Shows every protocol as % of total traffic
- Unexpected protocols (IRC, Telnet) = red flag
- Very high DNS % = possible DNS tunneling
- Very high ICMP % = possible tunneling or ping sweep

**2. Conversations** (`Statistics → Conversations → TCP tab`)
- Shows which hosts talked most and how much data transferred
- External IP with huge data transfer = exfiltration
- Many connections to single IP = C2 beaconing

**3. IO Graph** (`Statistics → IO Graph`)
- Shows traffic volume over time as a graph
- Regular spikes every X seconds = beaconing
- Sudden massive spike = data exfiltration burst

**4. DNS Statistics** (`Statistics → DNS`)
- Shows all DNS query types and response codes
- High NXDOMAIN count = DGA malware failing to find C2
- Many queries to same domain = beaconing

### 🔧 The Analyst Triage Workflow — Always This Order
1. NetworkMiner → fast triage, get host list, check Files + Credentials
2. Protocol Hierarchy → any unexpected protocols?
3. Conversations → who is talking most? External IPs?
4. IO Graph → any beaconing pattern?
5. DNS filter → suspicious C2 domains?
6. HTTP filter → suspicious POST requests?
7. Follow TCP streams on suspicious connections → read actual conversation

### 📡 Ethernet Frames and MAC Addresses
| Field | Size | Purpose |
|---|---|---|
| Destination MAC | 6 bytes | Who receives this frame |
| Source MAC | 6 bytes | Who sent this frame |
| Type | 2 bytes | IPv4=0x0800, ARP=0x0806 |
| Data (payload) | 46-1500 bytes | The actual IP packet |
| CRC | 4 bytes | Error detection |

**Key fact:** MAC addresses are Layer 2 — they only exist within a
local network. Every router hop replaces the MAC address but keeps
the IP address.

### 🛠️ Lab Output
**NetworkMiner (smtp.pcap):**
- Hosts tab: 6 hosts discovered
- OS fingerprinting: Hostname "GP", OS: Windows (TTL=128)
- Files: 3 files extracted
- Credentials: 1 credential found
- DNS: 2 DNS entries

**Wireshark Statistics:**
- Protocol Hierarchy: SMTP=53.3% (dominant!), TCP=88.3%, DNS=3.3%
- Conversations: 10.10.1.4 ↔ 74.53.140.153, 53 packets, 24KB
- DNS Stats: NS, CNAME, A records — 100% no error responses
- IO Graph: Clear traffic spike at ~2-4 seconds with TCP errors (red)

### 📌 Summary
- NetworkMiner = automatic extraction (first 5 minutes)
- Wireshark = deep manual analysis (the details)
- Use both together — never just one tool
- Credentials tab in NetworkMiner can reveal stolen passwords instantly

---
### 🚀 Progress
✔ Completed: NetworkMiner triage, Wireshark Statistics, triage workflow  
📅 Day 13: Professional analyst triage workflow mastered  
➡️ Next: TryHackMe — Wireshark Rooms (Guided Lab)


## 📘 Day 14 – TryHackMe Rooms

Today I completed 3 TryHackMe rooms — Offensive Security Intro,
Defensive Security Intro, and Careers in Cyber.

### ✅ Room 1 — Offensive Security Intro
**Concept:** Think like an attacker to find weaknesses before real
hackers do.

**Lab — FakeBank attack:**
- Found bank account number: 8881
- Used `dirb http://fakebank.thm` to enumerate hidden pages
- Found hidden admin page: `/bank-transfer`
- Accessed unprotected admin panel and completed deposit
- Flag: **BANK-HACKED**
- Fix: Require login to protect the bank-transfer page

**Key learning:** Hidden pages that aren't linked can still be found
by attackers using directory brute-forcing tools like `dirb`

### ✅ Room 2 — Defensive Security Intro
**Concept:** Detect, investigate, and respond to attacks before
damage occurs.

**Lab — SOC analyst simulation:**
- Opened monitoring dashboard as apprentice SOC analyst Joe
- Identified suspicious source IP: `32.122.195.63`
- Identified attack type: Web Discovery Attack (automated
  directory enumeration)
- Attack details: Started 14/07/2025, lasted 16 min 32 sec,
  31 URLs attempted, 10 blocked
- Blocked attacker IP via firewall rule
- Flag: **THM{FAKEBANK-SECURED}**

**Key learning:** SOC analysts monitor dashboards for suspicious
activity and respond with containment (blocking the attacker)

### ✅ Room 3 — Careers in Cyber
- Millions of cyber roles are unfilled — people from all
  backgrounds can fill them
- Certifications alone can get you a well-paying role faster
  than most expect
- No degree required — barrier to entry is lower than assumed
- 50+ recognised roles: penetration testers, SOC analysts,
  threat intelligence, incident responders, security engineers
- Cyber security is one of the fastest growing career fields

### 🏆 Results
- Total points earned: 120
- Rooms completed: 3
- Streak: 1 🔥

### 📌 Summary
- Offensive security = find weaknesses before attackers do
- Defensive security = detect, respond, contain attacks
- dirb = directory brute-forcing tool to find hidden pages
- SOC analysts rely on monitoring dashboards to detect threats
- Containment = stopping the attack while it's happening

---
### 🚀 Progress
✔ Completed: 3 TryHackMe rooms — offensive, defensive, careers  
📅 Day 14: First real hands-on cybersecurity labs completed  
➡️ Next: Windows Event Logs


## 📘 Day 15 – Windows Event Logs

Today I studied Windows Event Logs — the black box recorder of
every Windows system — and queried them live using both Event
Viewer and PowerShell.

### 🔑 What Are Windows Event Logs?
Windows records everything that happens on a system in Event Logs.
For a DFIR analyst these logs are like a black box recorder —
they tell you who logged in, what ran, what failed, and when.

### 📋 Event Log Locations
| Log | Path | Contains |
|---|---|---|
| Security | Windows Logs\Security | Logons, privilege use, object access |
| System | Windows Logs\System | OS events, service start/stop |
| Application | Windows Logs\Application | App crashes, errors |
| PowerShell | Apps & Services\Microsoft\Windows\PowerShell | PS commands run |
| Sysmon | Apps & Services\Microsoft\Windows\Sysmon | Deep system monitoring |

### 🔐 Authentication Events
| Event ID | Meaning | DFIR Relevance |
|---|---|---|
| 4624 | Successful logon | Who logged in and when |
| 4625 | Failed logon | Brute force attempts |
| 4634 | Logoff | Session ended |
| 4648 | Logon with explicit credentials | Lateral movement |
| 4672 | Special privileges assigned | Admin logon |
| 4768 | Kerberos ticket requested | Domain auth |
| 4771 | Kerberos pre-auth failed | Failed domain logon |

### 👤 Account Management Events
| Event ID | Meaning | DFIR Relevance |
|---|---|---|
| 4720 | User account created | Attacker creating backdoor account |
| 4722 | User account enabled | Dormant account activated |
| 4724 | Password reset attempt | Privilege escalation |
| 4728 | User added to security group | Privilege escalation |
| 4732 | User added to local admin group | Critical — attacker gaining admin |

### 🖥️ Process and Execution Events
| Event ID | Meaning | DFIR Relevance |
|---|---|---|
| 4688 | New process created | What ran on the machine |
| 4689 | Process exited | Process lifetime |
| 1102 | Audit log cleared | Attacker covering tracks! |
| 4698 | Scheduled task created | Persistence mechanism |
| 7045 | New service installed | Malware installing itself |

### 🔢 Logon Types — Inside Event 4624
| Type | Name | Meaning |
|---|---|---|
| 2 | Interactive | Local keyboard login |
| 3 | Network | File share, net use |
| 4 | Batch | Scheduled task |
| 5 | Service | Windows service |
| 7 | Unlock | Screen unlock |
| 10 | RemoteInteractive | RDP login ⚠️ |
| 11 | CachedInteractive | Offline cached login |

> Logon Type 10 = RDP — always investigate in an incident!

### 📖 Reading a 4624 Event
- Event ID: 4624
- Time: 2024-04-14 03:42:17
- Account Name: Abhishek
- Account Domain: DESKTOP-XYZ
- Logon Type: 10 ← RDP — suspicious at 3am!
- Source IP: 185.220.x.x ← External IP = very suspicious!
- Workstation: ATTACKER-PC

### ⚔️ Attack Timeline Using Event Logs
- 4625 × 50 → Brute force (50 failed logons)
- 4624 Type 3 → Network logon succeeded
- 4728 → Added to security group
- 4732 → Added to local admin group
- 4688 → Malicious process launched
- 4698 → Scheduled task created (persistence)
- 1102 → Logs cleared (covering tracks)

### 💻 PowerShell Event Logging
- Event 4103 → Pipeline execution
- Event 4104 → Script block logging — full command visible
- Event 4104 is gold — shows exactly what PowerShell command
  was run, even obfuscated ones

### 🛠️ Lab Output
**Event Viewer:**
- Found Event 4624 (Successful logon)
  - Account: DESKTOP-15109N0$ / SYSTEM
  - Logon Type: 5 (Service)
  - Date: 24-08-2026 09:18 PM
- Found Event 4625 (Failed logon)
  - Logon Type: 2 (Interactive)
  - Failure reason: Error occurred during logon
  - Date: 24-08-2026 09:18 PM

**PowerShell commands run:**
```powershell
Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4624} -MaxEvents 5
# → 5 successful logon events returned (09:18 PM - 09:27 PM)

Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4625} -MaxEvents 5
# → 5 failed logon events returned (oldest: 20-08-2026)

Get-EventLog -LogName Security -Newest 20
# → 20 events returned, mostly InstanceID 5379
#   (Credential Manager credentials read)
```

### 📌 Summary
- Windows Event Logs = black box recorder of the system
- 4624 + Logon Type 10 = RDP login — always investigate
- 1102 = attacker clearing their tracks
- PowerShell 4104 = most powerful forensic logging
- Use eventvwr.msc for GUI, Get-WinEvent for automation

---
### 🚀 Progress
✔ Completed: Event IDs, logon types, attack timeline, live lab  
📅 Day 15: Windows Event Log forensics mastered  
➡️ Next: C2 Traffic Patterns


## 📘 Day 16 – C2 Traffic Patterns

Today I studied C2 (Command & Control) traffic — how malware
communicates with its attacker after infecting a machine — and
learned the 5 core traffic patterns analysts look for, plus how
to detect each one in Wireshark.

### 🔑 What is C2 Traffic?
- C2 = how malware communicates with the attacker after infecting a machine
- Attacker sends commands TO the malware, malware sends stolen data BACK
- Flow: Victim's PC (Infected Machine) → C2 Server (attacker-controlled)
- C2 server is usually a VPS, compromised website, or DGA domain

### 🚩 The 5 C2 Traffic Patterns

**1) Beaconing**
- Malware checks in with its C2 server at regular intervals — like a heartbeat
- Example: 10:00:00 → 10:00:60 → 10:02:00 → 10:03:00 (exactly 60s apart)
- How to spot it: IO Graph shows regular spikes; Conversations tab shows many connections to one IP at equal intervals
- DFIR filter: `ip.dst == [suspicious IP]`

**2) DGA — Domain Generation Algorithm**
- Instead of hardcoding a C2 domain (which can be blocked), malware generates random domains daily from an algorithm only attacker & malware know
- Example: Day 1: xkq7z3mpa.ru / Day 2: mzp9qw2nx.com / Day 3: hbt5k18yz.net
- How to spot it: very long random-looking domain names; high NXDOMAIN count (malware trying many domains until it finds the live one); unusual TLDs (.ru, .tk, .xyz)
- DFIR filter: `dns and dns.flags.rcode==3` (NXDOMAIN responses)

**3) HTTP POST Exfiltration**
- Malware sends stolen data (keystrokes, files, credentials) to C2 using HTTP POST requests — same as submitting a web form, but the "form data" is your stolen information
- Example: `POST /gate.php HTTP/1.1`, Host: 185.220.x.x, User-Agent: Mozilla/4.0 (fake browser), Content: [encrypted stolen data]
- How to spot it: POST to unusual URIs (/gate.php, /panel, /b374k.php); POST to a bare IP address (no domain); unusual/fake User-Agent strings; large Content-Length in POST
- DFIR filter: `http.request.method=="POST"`

**4) HTTPS C2 (Encrypted Beaconing)**
- Modern malware uses HTTPS to hide C2 traffic in encrypted traffic — you can't see the content, but you can still see: the domain via SNI in the TLS Client Hello, regular connection timing (beaconing), certificate anomalies (self-signed, wrong domain)
- DFIR filter: `tls.handshake.type==1` (Client Hello — shows SNI)

**5) DNS Tunneling**
- Attacker hides data inside DNS queries — using subdomains to smuggle data out
- Normal DNS: query → google.com
- Tunneling: query → 546865774681696566.c2domain.com (hex-encoded stolen data)
- How to spot it: extremely long subdomain names (>50 chars); high volume of DNS queries to one domain; DNS query sizes much larger than normal (normal ≈ 30 bytes, tunneling ≈ 100+ bytes)
- DFIR filter: `dns.qry.name.len>50`

### 📋 C2 Traffic vs Normal Traffic

| Feature | Normal Traffic | C2 Traffic |
|---|---|---|
| DNS Queries | Known domains | Random/DGA domains |
| Timing | Irregular | Regular (beaconing) |
| HTTP POST | Form submissions | Stolen data |
| User-Agent | Real browser | Fake/unusual |
| Destination | CDNs, known servers | Unknown IPs |
| Volume | Varies | Consistent (beacon) |
| Certificates | Trusted CAs | Self-signed |

### 🦠 Real Malware C2 Patterns

| Malware | C2 Method | What to look for |
|---|---|---|
| Emotet | HTTP POST | POST to WordPress sites, fake User-Agent |
| Cobalt Strike | HTTPS beacon | Regular HTTPS every 60s, malleable C2 |
| Trickbot | HTTPS | Many IPs, certificate anomalies |
| Qakbot | HTTPS + DNS | DGA domains, HTTPS beaconing |
| IcedID | HTTPS | GET requests with encoded data in URL |
| AsyncRAT | TCP direct | Connection to port 4444 or 6606 |

### 🔑 Key DFIR Filters for C2 Hunting
- `dns.flags.rcode==3` → NXDOMAIN — DGA failure
- `http.request.method=="POST"` → Data exfiltration
- `http.user_agent` → Fake user agents
- `tls.handshake.type==1` → C2 domain via SNI
- `tcp.port==4444` → Metasploit C2
- `dns.qry.name.len>50` → DNS tunneling
- `ip.dst==x.x.x.x` → Traffic to specific C2 IP
- Frame contains "gate.php" → common C2 URL

### 🛠️ Lab Output
Followed the malware-traffic-analysis.net workflow:
- **Step 1 — DGA Domains:** Applied `dns` filter on a real PCAP, observed
  wpad.mshome.net, config.edge.skype.com, assets.adobedtm.com chains and
  an ICMP "Destination unreachable (Port unreachable)" response
- **Step 2 — Data Exfiltration:** Applied `http.request.method=="POST"`,
  found POST requests to `/api/set_agent?id=...&token=...` with
  `application/x-www-form-urlencoded` content
- **Step 3 — C2 over HTTP:** Applied `http` filter, followed the sequence
  of GET/POST requests including a suspicious `/favicon.ico` GET and a
  404 Not Found response mixed in with legitimate-looking traffic

### 📌 Summary
- 5 C2 patterns: Beaconing, DGA, HTTP POST exfiltration, HTTPS encrypted
  beaconing, DNS tunneling
- Each pattern has a distinct signature and a specific Wireshark filter
- C2-vs-normal comparison table + real malware family cheat sheet built
  for future investigations

---
### 🚀 Progress
✔ Completed: 5 C2 traffic patterns, detection filters, malware family reference, hands-on PCAP lab  
📅 Day 16: C2 traffic hunting skills built  
➡️ Next: Zeek — Network Logs That Investigators Actually Use

Day 17
