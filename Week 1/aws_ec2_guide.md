# ☁️ AWS EC2 Guide for DevOps Engineers

Amazon EC2 (Elastic Compute Cloud) provides resizable compute capacity in the cloud. It's a key resource for deploying applications, running scripts, and setting up backend servers in a DevOps workflow.

---

## 🚀 What is EC2?

EC2 lets you launch **virtual machines (instances)** with various configurations (OS, CPU, memory, etc.) to host applications, databases, monitoring tools, CI/CD runners, etc.

---

## 📌 Key EC2 Concepts

| Term               | Description |
|--------------------|-------------|
| **AMI**            | Amazon Machine Image (template for OS + software) |
| **Instance Type**  | Hardware configuration (e.g., t2.micro, t3.medium) |
| **Key Pair**       | SSH keys to access the instance |
| **Security Group** | Acts like a firewall for your EC2 instance |
| **Elastic IP**     | Static IP address you can attach to an EC2 instance |
| **User Data**      | Script that runs at launch (often used to auto-install software) |

---

## 🛠️ Steps to Launch an EC2 Instance

1. **Go to EC2 Dashboard**
2. Click **"Launch Instance"**
3. Choose AMI (e.g., Amazon Linux, Ubuntu)
4. Choose **Instance Type** (e.g., `t2.micro` for free tier)
5. Configure:
   - Network: Choose VPC and subnet
   - Add **User Data** script (optional)
6. Add storage (default is fine)
7. Add tags (e.g., `Name: DevServer`)
8. Configure **Security Group**:
   - Allow ports: `22` (SSH), `80/443` (HTTP/HTTPS), `8080` or others as needed
9. Choose or create a **Key Pair** for SSH access
10. Launch the instance 🚀

---

## 🔐 Example SSH Command

```bash
ssh -i "my-key.pem" ec2-user@<your-ec2-public-ip>
