# 🏢 Domain Controller Setup (Active Directory)

## 📌 Overview

This document explains the setup of a Domain Controller using Windows Server. The purpose of this step is to create a centralized authentication system using Active Directory Domain Services (AD DS).

The domain created in this lab is used to manage users, computers, and network resources in a centralized environment.

---

## 🖥️ Server Details

- Server Name: DC01  
- Operating System: Windows Server (2019/2022)  
- Role: Domain Controller + DNS Server  
- Domain Name: `company.local`  
- IP Address: `192.168.132.10`  

---

## 🎯 Objective

- Install Active Directory Domain Services (AD DS)
- Promote server to Domain Controller
- Create a new forest and domain
- Configure DNS automatically
- Enable centralized user management

---

## 🪜 Step-by-Step Setup

### 1️⃣ Set Static IP Address

Configure the server with a static IP:

- IP Address: `192.168.132.10`
- Subnet Mask: `255.255.255.0`
- Default Gateway: `192.168.132.2`
- Preferred DNS: `127.0.0.1`

---

### 2️⃣ Install Active Directory Role

1. Open **Server Manager**
2. Click **Add Roles and Features**
3. Select:
   - Role-based installation
4. Choose server: **DC01**
5. Select Role:
   - Active Directory Domain Services (AD DS)
6. Click **Install**

---

### 3️⃣ Promote Server to Domain Controller

After installation:

1. Click **Promote this server to a domain controller**
2. Select:
   - Add a new forest
3. Enter domain name:

company.local

4. Set Directory Services Restore Mode (DSRM) password
5. Click **Next → Install**
6. Server will restart automatically

---

### 4️⃣ Verify Domain Setup

After reboot:

- Login using:

company\Administrator


Check:

- Active Directory Users and Computers
- DNS Manager
- Domain name appears correctly

---

### 5️⃣ Create Organizational Units (OU)

Inside Active Directory:

- Create OUs:
- IT Department
- HR Department
- Users
- Computers

---

### 6️⃣ Create Domain Users

Example users:

- admin.user
- it.support
- test.user

Assign users to appropriate OUs and groups.

---

## 📸 Evidence (Screenshots)

Add screenshots here:

```markdown
![Domain Controller Setup](../screenshots/dc_setup.png)
![Active Directory Users](../screenshots/ad_users.png)
