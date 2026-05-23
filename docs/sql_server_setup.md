# 🗄️ SQL Server Installation + Backup/Restore

## 📌 Overview

This document provides a comprehensive guide for setting up Microsoft SQL Server on SQL01 and implementing a robust backup and restore strategy. SQL Server is a relational database management system that allows you to:

- **Store and manage relational data**
- **Create and manage multiple databases**
- **Backup and restore databases for disaster recovery**
- **Integrate with Active Directory** for authentication
- **Query databases using T-SQL**
- **Automate maintenance tasks** like backups and integrity checks

By the end of this guide, you'll have a fully operational SQL Server with automated backups and tested restore procedures.

---

## 🖥️ Server Details

| Property | Value |
|----------|-------|
| **Server Name** | `SQL01` |
| **Domain** | `company.local` |
| **IP Address** | `192.168.132.30` |
| **Operating System** | Windows Server 2019/2022 |
| **SQL Server Version** | SQL Server 2019/2022 Standard |
| **Database Engine** | Relational Engine |
| **Default Instance** | MSSQLSERVER |
| **Port** | 1433 (TCP/IP) |

---

## 🎯 Objectives

- ✅ Download and install SQL Server 2019/2022
- ✅ Install SQL Server Management Studio (SSMS)
- ✅ Configure SQL Server for Active Directory integration
- ✅ Create sample databases with tables and data
- ✅ Implement full backup strategy
- ✅ Create and verify backup files
- ✅ Practice database restore procedures
- ✅ Automate backups using SQL Server Agent and Task Scheduler
- ✅ Document backup and restore procedures

---

## 📋 Pre-Requisites

Before starting, ensure:

| Item | Status | Details |
|------|--------|---------|
| SQL01 joined to company.local domain | ✅ Required | Must be domain member |
| Network connectivity to DC01 | ✅ Required | For DNS and authentication |
| Administrator access on SQL01 | ✅ Required | Local or domain admin |
| At least 10GB free disk space | ✅ Required | For SQL Server installation |
| At least 20GB backup storage | ✅ Required | For backup files |
| Windows Server 2019/2022 | ✅ Required | Latest OS patches applied |
| .NET Framework 4.7+ | ✅ Recommended | Pre-installed on Windows Server |

---

## 🪜 Step-by-Step Setup

### Phase 1: Download SQL Server Installation Media

#### Step 1.1: Download SQL Server Evaluation Edition

1. **On any machine, open a web browser:**
   - Navigate to: `https://www.microsoft.com/en-us/sql-server/sql-server-downloads`

2. **Select "Download now" under "SQL Server 2022 Evaluation"**
   - Or choose "SQL Server 2019" if preferred

3. **You'll see options:**
   ```
   SQL Server 2022 Evaluation
   ├─ Express (Free, 10GB limit)
   ├─ Developer (Free for dev/test)
   ├─ Evaluation (180-day trial)
   └─ Standard/Enterprise (Licensed)
   ```

4. **Click "Download now"** for Evaluation edition
   - File: `SQLServer2022-x64-ENU.iso` (~2GB)

5. **Wait for download to complete** (may take 10-20 minutes on slower connections)

#### Step 1.2: Transfer Installation Media to SQL01

**Option A: Direct Download on SQL01**

1. **On SQL01, download the ISO directly:**
   - Open browser
   - Navigate to SQL Server downloads page
   - Download directly to SQL01

**Option B: Copy from Another Machine**

1. **Download ISO on another machine**
2. **Copy to SQL01 via:**
   - USB drive
   - Network share
   - Remote Desktop file transfer

#### Step 1.3: Mount or Extract ISO

1. **On SQL01, locate the ISO file:**
   - Right-click the ISO file
   - Select: **Mount** (Windows 10/Server 2019+)
   - Or: **Extract All** (if mounting not available)

2. **Virtual drive appears:**
   - Drive letter: (e.g., `D:\`)
   - Contains SQL Server installer files

---

### Phase 2: Install SQL Server

#### Step 2.1: Launch SQL Server Installer

1. **Open File Explorer on SQL01:**
   - Navigate to mounted drive (e.g., `D:\`)

2. **Double-click: `setup.exe`**
   - SQL Server Setup Wizard launches

3. **Setup Center appears:**
   ```
   ┌──────────────────────────────────────┐
   │ SQL Server Installation Center       │
   │                                      │
   │ Planning                             │
   │ ├─ System Configuration Checker      │
   │ ├─ Read release notes                │
   │ └─ View system requirements          │
   │                                      │
   │ Installation                         │
   │ ├─ New SQL Server standalone         │
   │ │  installation ← CLICK HERE         │
   │ ├─ Add features to existing          │
   │ └─ Repair installation               │
   └──────────────────────────────────────┘
   ```

#### Step 2.2: Run System Configuration Checker

1. **First, click: "System Configuration Checker"** (recommended)

2. **Checker validates your system:**
   ```
   ✓ OS version compatible
   ✓ .NET Framework installed
   ✓ Disk space available
   ✓ Processor meets requirements
   ✓ Memory meets requirements
   ```

3. **If all checks pass:**
   - Click **OK** to close
   - Proceed to Step 2.3

#### Step 2.3: Start Installation

1. **Click: "New SQL Server standalone installation"**

2. **Welcome page appears:**
   - Click **Next →**

3. **License Terms page:**
   - ☑ **Check: "I accept the license terms and conditions"**
   - Click **Next →**

4. **Microsoft Update page:**
   - ☑ Keep checked (recommended)
   - Click **Next →**

#### Step 2.4: Choose Installation Type

1. **Installation Type page appears:**
   ```
   Which SQL Server components do you want to install?
   
   ○ Perform a new installation of SQL Server 2022
   ← SELECT THIS
   
   ○ Add shared features to an existing SQL Server 2022 instance
   ```

2. **Select top option (new installation)**

3. **Click Next →**

#### Step 2.5: Feature Selection - CRITICAL STEP

1. **Feature Selection page shows:**
   ```
   Instance Features (for this SQL Server instance):
   
   ☑ Database Engine Services ✓ SELECT
   ☐ SQL Server Replication
   ☐ Machine Learning Services
   ☐ Language Extensions
   
   Shared Features (all SQL Server instances):
   ☑ SQL Server Management Tools (SSMS) ✓ SELECT
   ☑ SQL Server Data Tools
   ☑ Analysis Services
   ☐ Reporting Services
   ☐ Integration Services
   ```

2. **Ensure these are CHECKED:**
   - ☑ **Database Engine Services** (essential)
   - ☑ **SQL Server Management Tools** (for SSMS)

3. **You can leave others unchecked** for lab

4. **Click Next →**

#### Step 2.6: Named Instance Configuration

1. **Instance Configuration page appears:**
   ```
   Instance Details:
   
   Instance ID: MSSQLSERVER
   (default instance - good for lab)
   
   Instance root directory:
   C:\Program Files\Microsoft SQL Server
   
   Shared feature directory:
   C:\Program Files\Microsoft SQL Server
   ```

2. **Keep defaults:**
   - Instance ID: `MSSQLSERVER` (default instance)
   - Directory paths as shown

3. **Click Next →**

#### Step 2.7: Database Engine Configuration

1. **Server Configuration page appears:**
   ```
   Service Accounts:
   
   SQL Server Database Engine:
   Account Name: [MSSQLSERVER]
   Password: [_______________]
   
   SQL Server Agent:
   Account Name: [SQLSERVERAGENT]
   Password: [_______________]
   ```

2. **Configure Service Accounts:**

   **Option 1: Local System Account (Lab Simple)**
   ```
   For each service:
   ☑ Local System account ← SIMPLE (lab only)
   ```

   **Option 2: Domain Service Account (Recommended)**
   ```
   For each service:
   ○ This account:
   [company\sql.service]
   [password________]
   ← BETTER for production
   ```

3. **For this lab, use Local System**

4. **Click Next →**

#### Step 2.8: Database Engine Configuration - Authentication

1. **Database Engine Configuration page appears:**
   ```
   Authentication Mode:
   
   ○ Windows Authentication mode
   ← SELECT FOR SECURITY
   
   ○ Mixed Mode (SQL Server and Windows Authentication)
   - Enter SQL Server Admin Password:
   [___________________]
   ← USE IF YOU NEED SA ACCOUNT
   
   ○ SQL Server Authentication only
   ```

2. **Select Authentication Mode:**

   **Recommended: Windows Authentication**
   ```
   ○ Windows Authentication mode ← SELECT THIS
   (More secure, uses AD credentials)
   ```

   **Or Mixed Mode (if you need SQL login):**
   ```
   ○ Mixed Mode (SQL Server and Windows Authentication)
   Enter SA password: [StrongPassword123!]
   ← Must be strong: uppercase, lowercase, number, special char
   ```

3. **Add Windows User as Admin:**
   - Click **Add...**
   - Add: `company\Administrator` (domain admin)
   - This allows domain user to manage SQL Server

4. **Click Next →**

#### Step 2.9: TempDB Configuration

1. **TempDB Configuration page appears:**
   ```
   TempDB Data Directory:
   C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Data
   
   Number of TempDB data files:
   [4]
   
   TempDB Log Directory:
   C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\Log
   ```

2. **Keep defaults** for lab environment

3. **Click Next →**

#### Step 2.10: Review and Install

1. **Ready to Install page appears:**
   ```
   Review your choices:
   ✓ Database Engine Services
   ✓ SQL Server Management Tools
   ✓ Authentication: Windows Auth
   ✓ Instance: MSSQLSERVER
   
   Installation Path: C:\Program Files\Microsoft SQL Server\
   ```

2. **Click "Install"** to begin installation

3. **Installation Progress shows:**
   ```
   Installing SQL Server 2022...
   
   ████████░░ 75%
   
   [Feature 1] - Completed
   [Feature 2] - In Progress
   [Feature 3] - Pending
   ```

4. **Installation takes 10-20 minutes**
   - Don't close the window
   - Don't restart the computer

5. **Complete page appears:**
   ```
   Setup completed successfully
   
   ✓ Database Engine - Success
   ✓ SQL Server Management Tools - Success
   
   [Close]
   ```

6. **Click "Close"**

#### Step 2.11: Verify SQL Server Installation

1. **SQL Server services should be running**

2. **Open Services:**
   - Press `Windows Key + R`
   - Type: `services.msc`
   - Press **Enter**

3. **Look for these services:**
   ```
   Service Name              Status    Startup
   ─────────────────────────────────────────
   SQL Server (MSSQLSERVER)  Running   Auto
   SQL Server Agent          Running   Auto
   SQL Server Browser        Running   Auto
   ```

4. **All should show "Running"** ✅

---

### Phase 3: Install SQL Server Management Studio (SSMS)

SSMS is the GUI tool for managing SQL Server.

#### Step 3.1: Download SSMS

1. **On SQL01, open browser:**
   - Navigate to: `https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms`

2. **Click "Download SSMS"**
   - File: `SSMS-Setup-ENU.exe` (~600MB)

3. **Wait for download** (10-15 minutes)

#### Step 3.2: Install SSMS

1. **Double-click SSMS installer**

2. **Installation wizard launches:**
   ```
   SQL Server Management Studio Setup
   
   Install Location:
   C:\Program Files (x86)\Microsoft SQL Server Management Studio 19\
   
   [Install] button
   ```

3. **Click "Install"**

4. **Installation completes:**
   ```
   Setup has finished
   
   Click Finish to close this dialog
   ```

5. **Click "Finish"**

#### Step 3.3: Launch SSMS

1. **Open SQL Server Management Studio:**
   - Press `Windows Key`
   - Type: `ssms`
   - Press **Enter**

2. **SSMS launches:**
   ```
   ┌────────────────────────────────────┐
   │ Connect to Server                  │
   │                                    │
   │ Server name:                       │
   │ [SQL01] or [localhost]             │
   │                                    │
   │ Authentication:                    │
   │ ○ Windows Authentication ✓         │
   │ ○ SQL Server Authentication        │
   │                                    │
   │ [Connect]                          │
   └────────────────────────────────────┘
   ```

3. **Configuration:**
   - Server name: `SQL01` or `localhost`
   - Authentication: **Windows Authentication**
   - Click **Connect**

4. **Object Explorer opens:**
   ```
   ├─ SQL01 (your server)
   │  ├─ Databases
   │  │  ├─ master
   │  │  ├─ model
   │  │  ├─ msdb
   │  │  └─ tempdb
   │  ├─ Security
   │  ├─ Server Objects
   │  └─ Replication
   ```

5. **SQL Server is ready!** ✅

---

### Phase 4: Create Sample Databases

#### Step 4.1: Create CompanyDB Database

1. **In SSMS Object Explorer, right-click "Databases"**
   - Select: **New Database...**

2. **New Database dialog:**
   ```
   Database name: [CompanyDB]
   Owner: [sa]
   
   Database files:
   [Shows path and size info]
   ```

3. **Enter Database Name:**
   - Type: `CompanyDB`

4. **Click OK**

5. **Database created and appears in Object Explorer:**
   ```
   Databases
   ├─ master
   ├─ model
   ├─ msdb
   ├─ tempdb
   └─ CompanyDB ← NEW
   ```

#### Step 4.2: Create Tables in CompanyDB

1. **Expand CompanyDB:**
   - Click arrow next to CompanyDB

2. **Right-click "Tables"**
   - Select: **New Table...**

3. **Table Designer opens**

4. **Create Employee table:**
   ```
   Column Name    Data Type       Allow Nulls
   ──────────────────────────────────────────
   EmployeeID     int (primary)   ☐ (no)
   FirstName      nvarchar(50)    ☑ (yes)
   LastName       nvarchar(50)    ☑ (yes)
   Email          nvarchar(100)   ☑ (yes)
   Department     nvarchar(50)    ☑ (yes)
   HireDate       datetime        ☑ (yes)
   Salary         decimal(10,2)   ☑ (yes)
   ```

5. **Enter this data in the designer**

6. **Save Table:**
   - Press `Ctrl + S`
   - Name: `Employee`
   - Click OK

#### Step 4.3: Insert Sample Data

1. **Right-click Employee table**
   - Select: **Edit Top 200 Rows**

2. **Data grid appears**

3. **Insert sample records:**
   ```
   EmployeeID | FirstName | LastName | Email              | Department | HireDate   | Salary
   ───────────┼───────────┼──────────┼────────────────────┼────────────┼────────────┼───────
   1          | John      | Smith    | john@company.local | IT         | 2023-01-15 | 75000
   2          | Sarah     | Johnson  | sarah@company.local| HR         | 2023-02-20 | 65000
   3          | Mike      | Williams | mike@company.local | IT         | 2023-03-10 | 85000
   4          | Lisa      | Brown    | lisa@company.local | Finance    | 2023-04-05 | 72000
   ```

4. **Close table** (data auto-saves)

5. **Your database now has data!** ✅

---

### Phase 5: Configure SQL Server Backups

#### Step 5.1: Create Backup Directory

1. **On SQL01, open File Explorer:**
   - Press `Windows Key + E`

2. **Create backup folder:**
   - Navigate to: `C:\`
   - Right-click → **New** → **Folder**
   - Name: `SQLBackups`

3. **Full path:** `C:\SQLBackups`

4. **Set permissions:**
   - Right-click `SQLBackups` folder
   - Select **Properties** → **Security**
   - Add user: `NT SERVICE\MSSQLSERVER`
   - Permissions: ✅ **Full Control**

#### Step 5.2: Create Full Backup

1. **In SSMS, right-click CompanyDB**
   - Select: **Tasks** → **Back Up...**

2. **Back Up Database dialog:**
   ```
   Database: CompanyDB
   Backup type: ● Full ← SELECT
   
   Backup component:
   ○ Database
   ○ Files and filegroups
   
   Backup to:
   ○ Disk [Add] [Remove]
   ○ Tape
   
   [Backup file path]
   ```

3. **Configuration:**
   - Backup type: **Full**
   - Click **Add** to specify path

4. **Specify backup path:**
   ```
   Backup file name:
   [C:\SQLBackups\CompanyDB_Full_20260522.bak]
   ```

5. **Click OK**

6. **Backup starts:**
   ```
   Backing up database CompanyDB...
   
   ████████░░ 50%
   ```

7. **Success message:**
   ```
   The backup of database 'CompanyDB' has completed successfully.
   ```

8. **Backup file created:** ✅
   ```
   C:\SQLBackups\CompanyDB_Full_20260522.bak (~50MB typical)
   ```

---

### Phase 6: Verify Backup Integrity

#### Step 6.1: Check Backup File

1. **Open File Explorer:**
   - Navigate to: `C:\SQLBackups`

2. **You should see:**
   ```
   CompanyDB_Full_20260522.bak  (~50MB)
   [File exists] ✅
   ```

#### Step 6.2: Verify Backup with T-SQL

1. **In SSMS, open New Query:**
   - Click **New Query** button (top left)

2. **Paste this T-SQL script:**
   ```sql
   -- Verify backup file integrity
   RESTORE VERIFYONLY 
   FROM DISK = 'C:\SQLBackups\CompanyDB_Full_20260522.bak'
   GO
   ```

3. **Click "Execute"** (F5)

4. **Expected output:**
   ```
   The backup set is complete and all backup files are valid.
   ```

5. **If successful, backup is good!** ✅

---

### Phase 7: Practice Database Restore

#### Step 7.1: Create a Test Database

1. **Create test database to restore into:**
   - Right-click **Databases**
   - Select: **New Database**
   - Name: `CompanyDB_Test`
   - Click OK

#### Step 7.2: Perform Full Restore

1. **Right-click CompanyDB_Test**
   - Select: **Tasks** → **Restore** → **Database...**

2. **Restore Database dialog:**
   ```
   Database: CompanyDB_Test
   Source device: ● From device
   
   [... button to browse]
   ```

3. **Specify backup source:**
   - Click the [...] button
   - Navigate to: `C:\SQLBackups\CompanyDB_Full_20260522.bak`
   - Click **OK**

4. **Restore options:**
   ```
   Backup sets to restore:
   ☑ CompanyDB_Full_20260522.bak
   
   Restore to:
   (select backup set to restore)
   ```

5. **Click **Options** tab:**
   ```
   Recovery state:
   ○ RESTORE WITH RECOVERY (default) ← SELECT
   ○ RESTORE WITH NORECOVERY
   ○ RESTORE WITH STANDBY
   ```

6. **Click OK**

7. **Restore starts:**
   ```
   Restoring database CompanyDB_Test...
   ████████░░ 75%
   ```

8. **Success message:**
   ```
   Restore of database 'CompanyDB_Test' completed successfully.
   ```

#### Step 7.3: Verify Restored Database

1. **Expand CompanyDB_Test in Object Explorer**

2. **Verify tables:**
   - Tables → Employee table exists ✅

3. **Verify data:**
   - Right-click Employee table
   - Select: **Edit Top 200 Rows**
   - You see the 4 employee records ✅

4. **Restore successful!** ✅

---

### Phase 8: Automate Backup Scheduling

#### Step 8.1: Create SQL Server Agent Job

1. **In SSMS Object Explorer, expand:**
   - SQL Server Agent → Jobs (right-click)

2. **Select: "New Job..."**

3. **New Job dialog:**
   ```
   Name: [Daily CompanyDB Backup]
   Owner: [sa]
   Category: [Database Maintenance]
   Enabled: ☑ Yes
   ```

4. **Enter job name:**
   - Type: `Daily CompanyDB Backup`

#### Step 8.2: Create Job Step

1. **Click "Steps" page (left side)**

2. **Click "New..."** to add step

3. **New Job Step dialog:**
   ```
   Step name: [Backup CompanyDB]
   Type: [T-SQL script]
   ```

4. **Enter T-SQL backup command:**
   ```sql
   BACKUP DATABASE [CompanyDB]
   TO DISK = N'C:\SQLBackups\CompanyDB_Full_$(ESCAPE_DQUOTE(SQLLOGDIR))\CompanyDB_Full_$(ESCAPE_DQUOTE(DATE)).bak'
   WITH NOFORMAT, NOINIT, NAME = N'CompanyDB-Full Database Backup', SKIP, NOREWIND, NOUNLOAD, STATS = 10
   GO
   ```

5. **Or use simpler version:**
   ```sql
   BACKUP DATABASE [CompanyDB]
   TO DISK = 'C:\SQLBackups\CompanyDB_Backup.bak'
   WITH COMPRESSION, INIT, STATS = 10
   GO
   ```

6. **Click OK**

#### Step 8.3: Configure Job Schedule

1. **Click "Schedules" page**

2. **Click "New..."** to add schedule

3. **New Job Schedule:**
   ```
   Name: [Daily Backup 2AM]
   Enabled: ☑ Yes
   Frequency: ○ Daily ← SELECT
   
   Daily frequency:
   Every: [1] day
   
   Occurs once at: [02:00:00] ← 2 AM
   
   Start date: [5/22/2026]
   No end date: ☑ Yes
   ```

4. **Click OK**

4. **Click OK** on main Job dialog

5. **Job created!** ✅

#### Step 8.4: Verify Job

1. **Expand SQL Server Agent → Jobs**

2. **You see your job:**
   ```
   Daily CompanyDB Backup
   Status: Idle (runs at 2 AM)
   ```

3. **To test job manually:**
   - Right-click job
   - Select: **Start Job at Step...**
   - Click OK
   - Job runs immediately ✅

---

## 🧪 Verification Commands

### Test SQL Server Connectivity

```powershell
# Open PowerShell as Administrator on SQL01

# Test local connection
sqlcmd -S SQL01 -E

# If successful, you see:
# 1>
# (SQL prompt ready)

# Exit sqlcmd
# Type: exit
# Press Enter
```

### List SQL Server Instances

```powershell
Get-Service -Name "*SQL*"

# Should show:
# SQL Server (MSSQLSERVER)  - Running
# SQL Server Agent          - Running
# SQL Server Browser        - Running
```

### Check Backup Files

```powershell
# List backup files
Get-ChildItem -Path "C:\SQLBackups\" -Filter "*.bak"

# Should show:
# CompanyDB_Full_20260522.bak
```

### View SQL Server Logs

```powershell
# Open SQL Server error logs
Get-EventLog -LogName Application -Source "MSSQL*" -Newest 10

# Shows recent 10 SQL Server events
```

---

## 📊 Backup Best Practices

| Practice | Reason |
|----------|--------|
| **Regular Full Backups** | Baseline recovery point |
| **Differential Backups** | Faster backup after full |
| **Transaction Log Backups** | Point-in-time recovery |
| **Backup Compression** | Reduces storage space 50-80% |
| **Backup to Network** | Offsite backup location |
| **Test Restores** | Ensure backups work |
| **Monitor Job Logs** | Catch backup failures |
| **Multiple Backup Locations** | Redundancy for critical DBs |
| **Document Procedures** | Know how to restore |
| **Verify Integrity** | Use RESTORE VERIFYONLY |

---

## 🔐 Security Considerations

| Item | Configuration | Status |
|------|----------------|--------|
| **Authentication** | Windows Auth with Domain | ✅ Secure |
| **SA Account** | Disabled/Strong password | ✅ Protected |
| **Network Access** | Only from domain machines | ✅ Restricted |
| **Backup Permissions** | MSSQLSERVER service account | ✅ Controlled |
| **Backup Location** | Local C: drive or network | ✅ Monitored |
| **Admin Rights** | Domain admins only | ✅ Limited |

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| **Can't connect to SQL Server** | Verify SQL Server service is running: `Get-Service MSSQLSERVER` |
| **SSMS won't connect** | Check Windows Authentication; verify user is domain member |
| **Backup fails** | Check C:\SQLBackups permissions; verify MSSQLSERVER service account |
| **RESTORE VERIFYONLY fails** | Backup file is corrupted; recreate backup |
| **Agent job won't run** | Check SQL Server Agent service is running |
| **Low disk space** | Clean old backups; move backups to network location |
| **Restore hangs** | Check disk space; verify backup file integrity first |

---

## 📸 Evidence Screenshots

Add screenshots to document SQL Server setup:

| Screenshot | Purpose |
|-----------|---------|
| ![SQL Server Installation](../screenshots/sql_installation.png) | SQL Server installer - feature selection |
| ![SSMS Connected](../screenshots/ssms_connected.png) | SSMS showing connected server |
| ![CompanyDB Created](../screenshots/companydb_created.png) | CompanyDB database in Object Explorer |
| ![Employee Table Data](../screenshots/employee_table_data.png) | Sample data in Employee table |
| ![Backup Complete](../screenshots/backup_complete.png) | Successful backup completion message |
| ![Restore Complete](../screenshots/restore_complete.png) | Successful restore completion message |
| ![Backup Files](../screenshots/backup_files.png) | Backup files in C:\SQLBackups folder |

---

## ✅ Completion Checklist

- [ ] SQL Server 2022 downloaded
- [ ] SQL Server installed on SQL01
- [ ] SQL Server services running (Database Engine, SQL Agent, Browser)
- [ ] SSMS installed and working
- [ ] Connected to SQL Server via SSMS
- [ ] CompanyDB database created
- [ ] Employee table created with sample data
- [ ] C:\SQLBackups folder created with proper permissions
- [ ] Full backup of CompanyDB created
- [ ] Backup file exists: CompanyDB_Full_20260522.bak
- [ ] Backup verified with RESTORE VERIFYONLY
- [ ] CompanyDB_Test database created
- [ ] Full restore completed to CompanyDB_Test
- [ ] Restored database verified (tables and data present)
- [ ] SQL Server Agent job created for daily backups
- [ ] Job schedule configured for 2 AM daily
- [ ] Backup job tested manually and runs successfully
- [ ] PowerShell connectivity test passes
- [ ] SQL Server accessible from other domain machines (IIS01, CLIENT01)
- [ ] Documentation complete with screenshots

---

## 📚 T-SQL Scripts Reference

### Create Database
```sql
CREATE DATABASE CompanyDB
ON PRIMARY (
    NAME = CompanyDB_dat,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\DATA\CompanyDB.mdf',
    SIZE = 10MB,
    FILEGROWTH = 2MB
)
LOG ON (
    NAME = CompanyDB_log,
    FILENAME = 'C:\Program Files\Microsoft SQL Server\MSSQL16.MSSQLSERVER\MSSQL\DATA\CompanyDB_log.ldf',
    SIZE = 5MB,
    FILEGROWTH = 2MB
)
GO
```

### Full Backup
```sql
BACKUP DATABASE [CompanyDB]
TO DISK = 'C:\SQLBackups\CompanyDB_Full_20260522.bak'
WITH COMPRESSION, INIT, STATS = 10
GO
```

### Verify Backup
```sql
RESTORE VERIFYONLY 
FROM DISK = 'C:\SQLBackups\CompanyDB_Full_20260522.bak'
GO
```

### Restore Database
```sql
RESTORE DATABASE [CompanyDB_Test]
FROM DISK = 'C:\SQLBackups\CompanyDB_Full_20260522.bak'
WITH REPLACE, RECOVERY
GO
```

### Get Database Info
```sql
SELECT name, state_desc, recovery_model_desc, create_date
FROM sys.databases
WHERE name = 'CompanyDB'
GO
```

### List Backup History
```sql
SELECT 
    bs.database_name,
    bs.backup_type,
    bs.backup_start_date,
    bs.backup_finish_date,
    bs.backup_size,
    bmf.physical_device_name
FROM msdb.dbo.backupset bs
INNER JOIN msdb.dbo.backupmediafamily bmf 
    ON bs.media_set_id = bmf.media_set_id
WHERE bs.database_name = 'CompanyDB'
ORDER BY bs.backup_start_date DESC
GO
```

---

## 🚀 Quick Reference Commands

### PowerShell Commands
```powershell
# Start SQL Server
Start-Service -Name "MSSQLSERVER"

# Stop SQL Server
Stop-Service -Name "MSSQLSERVER"

# Check SQL Server status
Get-Service -Name "MSSQLSERVER"

# Start SQL Agent
Start-Service -Name "SQLSERVERAGENT"

# Test SQL connectivity
sqlcmd -S SQL01 -E -Q "SELECT @@VERSION"
```

### SQL Server Agent
```sql
-- View scheduled jobs
SELECT name, enabled, description FROM msdb.dbo.sysjobs

-- View job history
SELECT job_name, run_status, run_date FROM msdb.dbo.sysjobhistory

-- Disable job
EXEC msdb.dbo.sp_update_job @job_name='Daily CompanyDB Backup', @enabled=0

-- Enable job
EXEC msdb.dbo.sp_update_job @job_name='Daily CompanyDB Backup', @enabled=1
```

---

## 📝 Notes

- **Evaluation License:** 180-day limit; upgrade to licensed version for production
- **Backup Strategy:** Implement 3-2-1 rule (3 copies, 2 media types, 1 offsite)
- **Transaction Logs:** Enable for point-in-time recovery (not covered here)
- **Network Backups:** Move backups to network share (\\SERVER\Share) for redundancy
- **Testing:** Always test restores regularly to ensure recovery capability
- **Monitoring:** Check backup job logs weekly to catch failures early

---

**Last Updated:** 2026-05-22  
**Status:** ✅ Ready for Implementation  
**Version:** 1.0
