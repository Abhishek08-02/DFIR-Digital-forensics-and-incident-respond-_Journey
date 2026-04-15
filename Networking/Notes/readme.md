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
