# AWS 3-Tier Architecture using Ansible

## 📌 Project Overview

This project demonstrates a production-style 3-tier architecture deployed in AWS using Ansible as Infrastructure as Code (IaC).

The architecture separates the application into three logical layers:

- Presentation Layer (Web Tier)
- Application Layer (App Tier)
- Data Layer (RDS Database)

All infrastructure components including VPC, EC2, Security Groups, ALB, and RDS are provisioned using Ansible playbooks.

---

## 🏗 Architecture Diagram

Internet  
   ↓  
Application Load Balancer (Public Subnets)  
   ↓  
Web Tier – Apache Reverse Proxy (Public Subnets)  
   ↓  
App Tier – Flask Application (Private Subnets)  
   ↓  
RDS MySQL Database (Private Subnets)

---

## 🧱 Infrastructure Components

### Networking
- Custom VPC
- 3 Public Subnets (Multi-AZ)
- 3 Private Subnets (Multi-AZ)
- Internet Gateway
- NAT Gateway
- Route Tables

### Compute Layer
- Bastion Host (Public Subnet)
- 2 Web EC2 Instances (Apache)
- 2 App EC2 Instances (Flask)

### Load Balancing
- Application Load Balancer (ALB)
- Target Group with health checks

### Database Layer
- Amazon RDS MySQL
- DB Subnet Group
- Private access only

---

## 🔐 Security Design

- ALB allows HTTP (80) from Internet
- Web Tier accepts traffic only from ALB
- App Tier accepts traffic only from Web Tier
- Database allows MySQL (3306) only from App Tier
- SSH access restricted via Bastion Host
- Private instances have no public IP addresses

This ensures proper network isolation and reduced attack surface.

---

## ⚙️ Technologies Used

- AWS (EC2, VPC, ALB, RDS, Security Groups)
- Ansible
- Apache2
- Flask (Python)
- MySQL (RDS)

---

## 🚀 Deployment Steps

### 1️⃣ Setup Networking
```bash
ansible-playbook vpc_setup.yml
````

### 2️⃣ Setup Compute Layer

```bash
ansible-playbook bastion+pub+pri+instances.yml
```

### 3️⃣ Configure Application & Web Tier

```bash
ansible-playbook -i inventory.ini site.yml
```

### 4️⃣ Setup ALB

```bash
ansible-playbook alb_setup.yml
```

### 5️⃣ Setup RDS

```bash
ansible-playbook rds_setup.yml
```

---

## 🌐 Application Access

Once deployed, the application can be accessed using:

```
http://<ALB-DNS-Name>
```

The ALB distributes traffic to web servers, which proxy requests to the Flask application running on private EC2 instances.

---

## 🧠 Key DevOps Concepts Implemented

* Infrastructure as Code (IaC)
* Idempotent Playbooks
* Reverse Proxy Architecture
* Load Balancing
* Multi-AZ Deployment
* Network Segmentation
* Secure Database Connectivity
* Systemd Service Management
* Python Virtual Environment Deployment

---

## 📈 High Availability Design

* Deployed across multiple Availability Zones
* Load Balancer health checks
* Horizontal scalability supported

---

## 🔮 Future Improvements

* Implement Auto Scaling Groups
* Enable HTTPS using ACM
* Store DB credentials in AWS Secrets Manager
* Add Web Application Firewall (WAF)
* Use multiple NAT Gateways for high availability

---

## 👤 Author

**Subramanyam P**  
DevOps Engineer  
AWS | Ansible | Linux | Python

---

## 📬 Contact

For questions or clarifications, feel free to reach out.
