
# 🧠 Networking Commands Cheat Sheet

This cheat sheet includes basic to advanced networking commands helpful for DevOps, system administrators, or anyone learning computer networking.

---

## 🔧 Linux Networking Commands

### 1. `ifconfig` (or `ip a`)

> Show IP address and network interfaces.

```bash
ifconfig         # deprecated but still used
ip a             # recommended modern command
```

### 2. `ping`

> Check connectivity to another host.

```bash
ping google.com          # Pings Google's server
ping -c 4 8.8.8.8        # Sends 4 pings to 8.8.8.8
```

### 3. `traceroute`

> Show the route packets take to a network host.

```bash
traceroute google.com
```

### 4. `netstat`

> Show network connections, routing tables, etc.

```bash
netstat -tulnp
```

### 5. `ss`

> Display sockets (more modern than `netstat`).

```bash
ss -tuln             # Show listening ports
```

### 6. `dig`

> DNS lookup tool.

```bash
dig google.com
```

### 7. `nslookup`

> DNS queries (older, but useful).

```bash
nslookup google.com
```

### 8. `curl`

> Transfer data from or to a server.

```bash
curl https://api.github.com
```

### 9. `wget`

> Download files from the internet.

```bash
wget https://example.com/file.zip
```

### 10. `host`

> DNS lookup for a domain name.

```bash
host google.com
```

---

## 🖥 Windows Networking Commands

### 1. `ipconfig`

> View IP configuration.

```bash
ipconfig
ipconfig /all       # Show detailed info
ipconfig /flushdns  # Clear DNS cache
```

### 2. `ping`

> Same as in Linux.

```cmd
ping google.com
```

### 3. `tracert`

> Windows version of `traceroute`.

```cmd
tracert google.com
```

### 4. `netstat`

> View network statistics.

```cmd
netstat -an
```

### 5. `nslookup`

> DNS lookup tool.

```cmd
nslookup google.com
```

### 6. `arp`

> View the ARP table (mapping of IP to MAC).

```cmd
arp -a
```

### 7. `telnet`

> Test port connectivity (if installed).

```cmd
telnet google.com 80
```

---

## 🔒 Ports and Protocols

| Protocol | Port  | Description            |
| -------- | ----- | ---------------------- |
| HTTP     | 80    | Web traffic            |
| HTTPS    | 443   | Secure web traffic     |
| SSH      | 22    | Secure shell access    |
| FTP      | 21    | File Transfer Protocol |
| DNS      | 53    | Domain Name System     |
| SMTP     | 25    | Email sending          |
| DHCP     | 67/68 | Dynamic IP assignment  |
| MySQL    | 3306  | Database access        |

---

## 🧪 Bonus: Check Open Ports

### Linux

```bash
sudo lsof -i -P -n | grep LISTEN
```

### Windows

```cmd
netstat -ab
```

---

## 🌐 Useful Tools

* **nmap** – Network scanner

  ```bash
  nmap -sP 192.168.1.0/24
  ```

* **tcpdump** – Packet sniffer

  ```bash
  sudo tcpdump -i eth0
  ```

* **Wireshark** – GUI-based network packet analyzer

Wireshark is a powerful packet analysis tool with a graphical interface. You can start it from the terminal:

```bash
sudo wireshark
```