# 🌐 Common Protocols and Port Numbers for DevOps

Understanding protocols and their default ports is critical in DevOps for managing infrastructure, CI/CD pipelines, firewalls, cloud security groups, and networking.

---

## ✅ Frequently Used Protocols

| Protocol | Port | Transport | Used For / DevOps Context |
|----------|------|-----------|----------------------------|
| HTTP     | 80   | TCP       | Web servers, REST APIs    |
| HTTPS    | 443  | TCP       | Secure APIs, websites     |
| SSH      | 22   | TCP       | Server access, Git over SSH |
| FTP      | 21   | TCP       | File transfers (less common today) |
| SFTP     | 22   | TCP       | Secure file transfers via SSH |
| DNS      | 53   | UDP/TCP   | Domain name resolution    |
| SMTP     | 25   | TCP       | Outgoing email (mail servers) |
| IMAP     | 143  | TCP       | Reading emails            |
| POP3     | 110  | TCP       | Receiving emails          |
| Telnet   | 23   | TCP       | Remote shell (legacy/insecure) |
| RDP      | 3389 | TCP       | Windows remote desktop    |
| MySQL    | 3306 | TCP       | MySQL/MariaDB databases   |
| PostgreSQL | 5432 | TCP     | PostgreSQL DB connections |
| MongoDB  | 27017| TCP       | NoSQL database access     |
| Redis    | 6379 | TCP       | In-memory data store      |
| Elasticsearch | 9200 | TCP  | REST API for search engine |
| Kafka    | 9092 | TCP       | Event streaming platform  |
| Docker Daemon | 2375 / 2376 | TCP | Remote Docker management |
| Prometheus | 9090 | TCP     | Metrics collection        |
| Grafana  | 3000 | TCP       | Monitoring dashboards     |
| Jenkins  | 8080 | TCP       | CI/CD pipelines           |
| Kubernetes API | 6443 | TCP | K8s cluster management    |

---

## 🔒 Security Tip
- Always use **firewall rules / security groups** to limit port access.
- Prefer **SSH, HTTPS, SFTP** over their insecure counterparts (Telnet, HTTP, FTP).
- Use **TLS certificates** to secure services like Prometheus, Grafana, Jenkins, etc.

---

## 📌 Use in Cloud
In AWS / Azure / GCP:
- These ports need to be **opened in Security Groups or Network Rules** for communication between services (e.g., EC2, RDS, Load Balancers, EKS nodes).

---

## 🧠 Helpful for:
- Setting up **CI/CD pipelines**
- Configuring **monitoring tools**
- Managing **cloud environments**
- **Container orchestration** (e.g., Docker, Kubernetes)
