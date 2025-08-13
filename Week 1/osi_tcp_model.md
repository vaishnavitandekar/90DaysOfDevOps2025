
# OSI & TCP/IP Models

## OSI Model (7 Layers)
1. **Application Layer** - Interface for end-user processes and applications. Examples: HTTP, DNS
2. **Presentation Layer** - Data translation, encryption, compression
3. **Session Layer** - Establishes, maintains, and ends communication sessions
4. **Transport Layer** - Ensures complete data transfer. Protocols: TCP, UDP
5. **Network Layer** - Handles routing and forwarding. Protocols: IP
6. **Data Link Layer** - Node-to-node data transfer. Protocols: Ethernet
7. **Physical Layer** - Transmission of raw bit stream over physical medium

## TCP/IP Model (4 Layers)
1. **Application Layer** - Corresponds to OSI's Application, Presentation, Session
2. **Transport Layer** - Ensures end-to-end communication. Protocols: TCP, UDP
3. **Internet Layer** - Handles packet routing. Protocols: IP
4. **Network Access Layer** - Corresponds to OSI's Data Link + Physical Layers

## 🌐 Real-World Examples

| OSI Layer       | TCP/IP Layer     | Protocols / Tools / DevOps Context           |
|----------------|------------------|----------------------------------------------|
| Application    | Application      | HTTP, HTTPS, FTP, DNS, REST APIs, cURL       |
| Presentation   | Application      | TLS/SSL, JSON, XML, YAML, JWT                |
| Session        | Application      | SSH, RPC, gRPC, WebSockets                   |
| Transport      | Transport        | TCP, UDP, Load Balancers, NGINX, HAProxy     |
| Network        | Internet         | IP, ICMP, NAT, VPN, Firewalls (iptables)     |
| Data Link      | Network Access   | Ethernet, MAC address, VLANs                 |
| Physical       | Network Access   | Cables, Switches, NICs, Cloud Infrastructure |
