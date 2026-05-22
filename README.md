# enterprise-homelab-infrastructure

# 🏠 Home Lab Infrastructure Portfolio

## 📌 Project Overview

This project demonstrates the design and implementation of a small enterprise-level IT infrastructure using virtualization. It includes the setup of a Windows Server environment, Active Directory domain services, a Linux monitoring server, SQL Server database management, and an IIS-hosted web server.

The goal is to simulate a real-world IT infrastructure and strengthen practical system administration, networking, and server management skills.

---

## 🎯 Objectives

- Build a multi-server virtual environment
- Configure Active Directory Domain Services (AD DS)
- Host a website using IIS
- Set up and manage SQL Server databases
- Implement Linux server monitoring tools
- Perform SQL backup and restore operations
- Practice VMware-based virtualization and networking

---

## 🧰 Technologies Used

- VMware Workstation
- Windows Server (2019/2022)
- Ubuntu Linux Server
- Microsoft SQL Server
- Internet Information Services (IIS)
- Active Directory Domain Services (AD DS)
- DNS & Static IP Networking

---

## 🖥️ Virtual Machine Architecture

| Machine Name | Role | Operating System |
|--------------|------|------------------|
| DC01 | Domain Controller (AD DS + DNS) | Windows Server |
| IIS01 | IIS Web Server | Windows Server |
| SQL01 | Database Server | Windows Server |
| LINUX01 | Monitoring Server | Ubuntu Linux |
| CLIENT01 | Domain Client Machine | Windows 10/11 |

---

## 🌐 Network Configuration

- Network Type: Host-Only / Internal Network (VMware)
- IP Range: `192.168.132.0/24`
- Domain: `company.local`

Example IP Assignments:

| Machine | IP Address |
|--------|------------|
| DC01 | 192.168.132.10 |
| IIS01 | 192.168.132.11 |
| SQL01 | 192.168.132.12 |
| LINUX01 | 192.168.132.13 |
| CLIENT01 | DHCP / Static |

---

## 🪜 Project Setup Steps

### 1️⃣ VMware Setup
- Install VMware Workstation
- Create Host-Only or Internal Network
- Configure IP range for lab communication

---

### 2️⃣ Domain Controller (DC01)
- Install Windows Server
- Assign static IP
- Install Active Directory Domain Services
- Promote server to Domain Controller
- Create domain: `company.local`

---

### 3️⃣ Domain Users & Groups
- Create Organizational Units (OU)
- Create users (Admin, IT, Standard Users)
- Assign group policies (GPO basics)

---

### 4️⃣ Join Machines to Domain
- Configure DNS to point to DC01
- Join IIS01, SQL01, CLIENT01 to domain

---

### 5️⃣ IIS Web Server (IIS01)
- Install Web Server (IIS)
- Create a website in:
- Host a simple HTML page
- Test via browser:


---

### 6️⃣ SQL Server Setup (SQL01)
- Install Microsoft SQL Server
- Install SQL Server Management Studio (SSMS)
- Create database:
- `CompanyDB`
- Create sample table:
- Users (ID, Name, Role)

---

### 7️⃣ SQL Backup & Restore
- Perform full database backup (.bak file)
- Restore database on same or different instance
- Verify data integrity after restore

---

### 8️⃣ Linux Monitoring Server (LINUX01)
- Install Ubuntu Server
- Install monitoring tools:
  bash
  sudo apt update
  sudo apt install htop net-tools -y

  Monitor:
    CPU usage
    RAM usage
    Network statistics

9️⃣ Domain Testing (CLIENT01)

  - Login using domain credentials:
      company\username

  - Test access to:
      - IIS website
      - File sharing
      - SQL connectivity
   

  📸 Screenshots

All important steps are documented with screenshots:

Domain Controller setup
Active Directory configuration
IIS hosted website
SQL Server database creation
Linux monitoring output
VM network configuration


📁 Repository Structure
Home-Lab-Project/
│
├── README.md
├── screenshots/
│   ├── dc_setup.png
│   ├── ad_users.png
│   ├── iis_website.png
│   ├── sql_database.png
│   ├── linux_monitoring.png
│
├── docs/
│   ├── domain_setup.md
│   ├── iis_setup.md
│   ├── sql_setup.md
│
└── network_diagram.png

🚀 Key Skills Demonstrated
Windows Server Administration
Active Directory Management
Web Server Hosting (IIS)
Database Administration (SQL Server)
Linux System Monitoring
Virtualization (VMware)
Networking & DNS Configuration
Backup & Restore Operations

📈 Outcome

This project simulates a real enterprise IT environment and demonstrates hands-on experience in system administration, infrastructure setup, and server management.

📌 Future Improvements
Add centralized logging (SIEM basics)
Implement automated backups
Add firewall and security hardening
Deploy monitoring dashboard (Prometheus/Grafana)
Introduce PowerShell automation scripts

👨‍💻 Author

Ahsan Abdullah
IT Support / System Administration Learning Path


---

If you want next upgrade, I can also create:

✔ :contentReference[oaicite:0]{index=0}  
✔ :contentReference[oaicite:1]{index=1}  
✔ :contentReference[oaicite:2]{index=2}  
✔ Or :contentReference[oaicite:3]{index=3}  

Just tell me 👍
