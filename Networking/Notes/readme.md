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
