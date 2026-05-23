# 🗄️ SQL Server Installation + Backup/Restore Documentation

## 📌 Overview

This document explains the installation and configuration of Microsoft SQL Server within the enterprise home lab environment. The SQL Server instance acts as the backend database server for the IIS web application and supports database backup and restore operations for disaster recovery testing.

---

## 🖥️ Server Information

- Server Name: SQL01  
- Operating System: Windows Server 2019/2022  
- Database Engine: Microsoft SQL Server 2022 Express  
- Management Tool: SQL Server Management Studio (SSMS)  
- IP Address: `192.168.100.12`  

---

## 🎯 Objectives

- Install Microsoft SQL Server 2022 Express
- Install SQL Server Management Studio (SSMS)
- Configure SQL Server network connectivity
- Create and manage databases
- Perform database backup and restore operations
- Validate connectivity between IIS and SQL Server

---

## 🪜 SQL Server Installation

### 1️⃣ Download SQL Server

Download:
- SQL Server 2022 Express
- SQL Server Management Studio (SSMS)

---

### 2️⃣ Install SQL Server

1. Run SQL Server installer
2. Select:
   ```
   Basic Installation
   ```
   or
   ```
   Custom Installation
   ```
3. Install Database Engine Services
4. Select Authentication Mode:
   - Windows Authentication
5. Complete installation

---

### 3️⃣ Install SSMS

1. Run SSMS installer
2. Complete installation
3. Launch SQL Server Management Studio
4. Connect to:
   ```
   SQL01
   ```

---

## ⚙️ SQL Server Configuration

### Enable TCP/IP Connectivity

1. Open:
   ```
   SQL Server Configuration Manager
   ```
2. Navigate:
   ```
   SQL Server Network Configuration
   → Protocols for SQLEXPRESS
   ```
3. Enable:
   - TCP/IP
4. Restart SQL Server service

---

### Configure SQL Port

Configured SQL Server to communicate using:

```plaintext
Port: 1433
```

---

### Configure Windows Firewall

Allowed inbound traffic for:
- SQL Server port 1433
- SQL Browser service

---

## 🗄️ Database Creation

### Create Lab Database

Database Name:
```sql
LabDB
```

---

### Create Sample Table

```sql
CREATE TABLE Users (
    ID INT PRIMARY KEY,
    Name VARCHAR(100),
    Role VARCHAR(50)
);
```

---

### Insert Sample Data

```sql
INSERT INTO Users VALUES
(1, 'Admin User', 'Administrator'),
(2, 'Test User', 'Employee');
```

---

## 🌐 IIS to SQL Connectivity Testing

Validated communication between:
- IIS web server (IIS01)
- SQL backend server (SQL01)

Performed:
- Database connection testing
- SQL authentication verification
- End-to-end application connectivity checks

---

## 💾 Database Backup Procedure

### Create Backup

1. Open SSMS
2. Right-click:
   ```
   LabDB
   → Tasks
   → Backup
   ```
3. Backup Type:
   - Full
4. Save backup file:
   ```
   LabDB.bak
   ```

---

## ♻️ Database Restore Procedure

### Restore Database

1. Open SSMS
2. Right-click:
   ```
   Databases
   → Restore Database
   ```
3. Select:
   ```
   Device → LabDB.bak
   ```
4. Start restore process
5. Verify restored database functionality

---

## 📸 Screenshots

Add screenshots here:

```markdown
![SQL Installation](../screenshots/sql_installation.png)
![SSMS Database](../screenshots/sql_database.png)
![SQL Backup](../screenshots/sql_backup.png)
![SQL Restore](../screenshots/sql_restore.png)
```

---

## 🧠 Key Learnings

- SQL Server installation and configuration
- Database management using SSMS
- TCP/IP and firewall configuration for SQL connectivity
- Database backup and recovery procedures
- Multi-tier application communication testing

---

## 🚀 Outcome

Successfully deployed and configured Microsoft SQL Server 2022 Express within a virtual enterprise lab environment. Validated secure communication between the IIS front-end and SQL backend while implementing backup and restore procedures for database recovery testing.

---

## 👨‍💻 Next Steps

- Configure SQL authentication accounts
- Implement scheduled backups
- Introduce SQL maintenance plans
- Connect SQL database to web applications
- Implement database monitoring and alerts
