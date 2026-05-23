# 🐧 Linux Monitoring Server Documentation

## 📌 Overview

This document explains the deployment and configuration of the Linux Monitoring Server within the enterprise home lab environment. The Linux server was configured to monitor system performance, resource utilization, and network connectivity across multiple Windows-based servers inside the virtual infrastructure.

The monitoring server provides visibility into server health, uptime, and communication between infrastructure components.

---

## 🖥️ Server Information

- Server Name: LINUX01  
- Operating System: Ubuntu Server 22.04 LTS  
- Role: Monitoring & Diagnostics Server  
- IP Address: `192.168.100.13`  

---

## 🎯 Objectives

- Deploy a Linux monitoring server in VMware
- Configure Linux networking
- Monitor system performance and resource usage
- Test connectivity between Windows and Linux systems
- Validate communication across the virtual infrastructure
- Perform troubleshooting and diagnostics

---

## 🪜 Ubuntu Server Installation

### 1️⃣ Create Virtual Machine

Inside VMware Workstation Pro:

- Create new virtual machine
- Select Ubuntu Server ISO
- Configure:
  - CPU: 2 Cores
  - RAM: 2–4 GB
  - Disk: 20 GB

---

### 2️⃣ Install Ubuntu Server

During installation:

- Configure hostname:
  ```
  LINUX01
  ```
- Configure static IP:
  ```
  192.168.100.13
  ```
- Install OpenSSH Server:
  ```
  ✔ Install OpenSSH Server
  ```

---

## ⚙️ Linux Network Configuration

### Verify Network Settings

Run:

```bash
ip a
```

---

### Test Connectivity

Verify communication with Windows servers:

```bash
ping 192.168.100.10
ping 192.168.100.11
ping 192.168.100.12
```

Test DNS resolution:

```bash
ping google.com
```

---

## 📊 Monitoring Tools Installation

### Update System

```bash
sudo apt update && sudo apt upgrade -y
```

---

### Install Monitoring Utilities

```bash
sudo apt install htop net-tools sysstat curl -y
```

Installed tools:
- htop → CPU & RAM monitoring
- net-tools → Network diagnostics
- sysstat → Performance statistics
- curl → Connectivity testing

---

## 📈 System Monitoring

### Monitor CPU & Memory Usage

```bash
htop
```

---

### Monitor Disk Usage

```bash
df -h
```

---

### Monitor RAM Usage

```bash
free -m
```

---

### Monitor Network Interfaces

```bash
ifconfig
```

or

```bash
ip a
```

---

## 🌐 Cross-Platform Connectivity Testing

Validated communication between:

- Linux Monitoring Server (LINUX01)
- Domain Controller (DC01)
- IIS Web Server (IIS01)
- SQL Server (SQL01)

Performed:
- Ping tests
- DNS verification
- Network troubleshooting
- Service availability checks

---

## 🔥 Windows Firewall Troubleshooting

Resolved communication issues between Linux and Windows servers by:

- Adjusting Windows Firewall rules
- Allowing ICMP traffic
- Enabling required service ports
- Verifying network profiles

Used PowerShell commands to troubleshoot and restore communication between systems.

---

## 📸 Screenshots

Add screenshots here:

```markdown
![Ubuntu Server](../screenshots/linux_server.png)
![HTOP Monitoring](../screenshots/htop_monitoring.png)
![Disk Usage](../screenshots/linux_disk_usage.png)
![Network Testing](../screenshots/linux_ping_test.png)
```

---

## 🧠 Key Learnings

- Linux server deployment and administration
- Network troubleshooting in virtual environments
- Cross-platform communication between Linux and Windows
- System resource monitoring and diagnostics
- VMware virtual networking configuration

---

## 🚀 Outcome

Successfully deployed and configured a Linux-based monitoring server capable of monitoring system health, testing connectivity, and supporting diagnostics across a multi-tier enterprise lab environment.

---

## 👨‍💻 Next Steps

- Install Prometheus Node Exporter
- Configure Grafana dashboards
- Implement centralized logging
- Add automated monitoring alerts
- Deploy advanced Linux monitoring tools
