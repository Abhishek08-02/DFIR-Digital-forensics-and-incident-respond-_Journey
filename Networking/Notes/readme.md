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

➡️ Next: Protocols (TCP vs UDP in depth)
