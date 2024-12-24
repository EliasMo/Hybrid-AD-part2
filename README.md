
# Hybrid AD Implementation - Part2: Cloud Integration 

## Prerequisites 
-  Completed Part 1 infrastructure
-  Azure Subscription
-  Global admin access to Azure/Microsoft 365
-  Windows Server 2022 Domain Controller

## 1. Download Azure AD Connect / Microsoft Entra Connect Sync 
### 1.1 Download and Installation 
- Downloaded Microsoft Entra Connect Sync
- Started Express Settings configuration

![image](https://github.com/user-attachments/assets/1df0375c-6c46-4792-a4ef-255998fe944d)

![image](https://github.com/user-attachments/assets/435d5055-b4d0-4217-8ed7-a2c8af954916)


### 1.2 Configuration Steps

- Global Administrator is ticked for Administrative roles 

![image](https://github.com/user-attachments/assets/d3671540-1d38-41ed-9aea-7727f035a4dc)


- Connect to Microsoft Entra and Active Directory Domain Services

![image](https://github.com/user-attachments/assets/59b839b3-967c-4b1a-ae6e-23fd67e20626)

- Click the Checkbox - "Continue without matching all UPN suffixes to verified domains"
![image](https://github.com/user-attachments/assets/091ca091-2afd-432d-a90f-87851691d4db)

- Configuration Completed

![image](https://github.com/user-attachments/assets/5d269f85-6d7e-4c37-bf0a-a68cffa6ebb5)

- Go back to Microsoft Entra and check if the users are there

![image](https://github.com/user-attachments/assets/5718b886-42bb-4d58-bb0e-c4c6acfd00cc)

### Troubleshooting and Downloading issues
- Had issues downloading Entra Connect

![image](https://github.com/user-attachments/assets/44e87bb0-f064-4d0b-a6a4-bbbed08d0ff3)

Used this website to fix the TLS 1.2 issue: 
https://learn.microsoft.com/en-us/entra/identity/hybrid/connect/reference-connect-tls-enforcement

- Internet Explorer Enhanced Security Configuration (IE ESC)
    - Had to disable through Server Manager
    - Set to "Off" for Admins
    - Go to Local Server then IE Enhanced Security Configuration

![image](https://github.com/user-attachments/assets/98656ed8-2f4d-4aa8-b736-51da0bb649d9)

- Make sure Global Administrator is ticked

![image](https://github.com/user-attachments/assets/59189857-b4c8-4c08-af80-ee68aabe9eb3)


## SQL Server Configuration 
### 2.1 Download and Installation 
- SQL Server 2022 Express setup

![image](https://github.com/user-attachments/assets/a651ae07-c600-48e8-b56b-41147f35f4e0)

- Run SQL Server Installation Center and choose Basic

![image](https://github.com/user-attachments/assets/ee055bb3-7128-4418-a4f9-b7d475e9db7c)

- Install SSMS: this will be used to manage the SQL Server

![image](https://github.com/user-attachments/assets/90709a34-2e03-4c12-842e-ccfd443ca63f)

![image](https://github.com/user-attachments/assets/b08a2156-644f-4d88-9738-16d1bb499da0)

SMSS will be used for: Creating the databases, security and permissions , writing queries and testing them, monitoring database performance, managing backups.

- Restart and go to SQL Management Studio

  Use the settings: Server type: Database Engine, localhost\SQLEXPRESS,Authentication: Windows Authentication.

  
![image](https://github.com/user-attachments/assets/e63d0e35-661f-4ed3-a402-ad2f88029d7a)

- If you have SSL certificate error - you can fix by clicking the trust server certificate box

![image](https://github.com/user-attachments/assets/420f40bb-80ac-4620-be4d-6bc428710131)


### 2.2 Database
- Naming the database name as HybridADManagment

  ![image](https://github.com/user-attachments/assets/d9504797-6752-48a2-9448-8ddcf5d00108)

- Create Query and create ADUserSync table

![image](https://github.com/user-attachments/assets/76c3316d-740a-4820-9370-284866fb4271)

This is the code if you want to try:

```sql
USE HybridADManagement
GO


CREATE TABLE ADUserSync (
	UserID INT IDENTITY(1,1) PRIMARY KEY,
	SamAccountName NVARCHAR(50) NOT NULL,
	UserPrincipalName NVARCHAR(100) NOT NULL,
	Email NVARCHAR(100),
	Department NVARCHAR(50),
	LastSyncTime DATETIME,
	SyncStatus NVARCHAR(20),
	AzureObjectID NVARCHAR(100),
	LastModifiedDate DATETIME
)
```

- Create ADSyncLogs table 

![image](https://github.com/user-attachments/assets/e7fcbef4-05c1-4e2d-b930-3ba2dd3146b3)

```sql
USE HybridADManagement
GO

CREATE TABLE ADSyncLogs (
	LogID INT IDENTITY (1,1) PRIMARY KEY,
	LogTime DATETIME DEFAULT GETDATE(),
	LogLevel NVARCHAR(10),
	LogMessage NVARCHAR(MAX),
	Source NVARCHAR(50)
)
```

- User creation for testing

![image](https://github.com/user-attachments/assets/17982920-9876-4939-9906-ad0736e8795f)

 
```sql
USE HybridADManagement
GO

-- Test--
INSERT INTO ADUserSync
(SamAccountName, UserPrincipalName, Email, Department, LastSyncTime, SyncStatus, AzureObjectID, LastModifiedDate)
VALUES
('jsmith', 'john.smith@internalcompany.com', 'john.smith@internalcompany.com', 'IT', GETDATE(), 'Synced', 'AAD-123-456', GETDATE()),
('awhite', 'alice.white@internalcompany.com', 'alice.white@internalcompany.com', 'HR', GETDATE(), 'PendingSync', 'AAD-789-012', GETDATE());


INSERT INTO ADSyncLogs 
(LogLevel, LogMessage, Source)
VALUES
('INFO', 'Initial sync completed for jsmith', 'Azure AD Connect'),
('WARNING', 'Sync pending for awhite', 'Azure AD Connect');
```
  
![image](https://github.com/user-attachments/assets/6c9cf7be-b9ec-46b8-a79c-64c02e35afe1)

# Project Status and Summary

## Completed 

### 1. Microsoft Entra Connect Setup
- Successfully installed Microsoft Entra Connect Sync
- Configured with Express Settings
- Established connection between on-premises AD and Azure AD
- Resolved initial configuration challenges:
  - TLS 1.2 implementation
  - IE Enhanced Security settings
  - Global Administrator permissions

### 2. SQL Server Implementation
- Installed SQL Server 2022 Express Edition
- Configured SQL Server Management Studio (SSMS)
- Created HybridADManagement database
- Implemented key database components:
  - ADUserSync table for user synchronization tracking
  - ADSyncLogs table for monitoring and auditing
  - Basic test

## Future
- Monitoring and alerting setup
- Integration with additional service

## Used
- Windows Server 2022
- SQL Server 2022 Express
- Microsoft Entra Connect
- Azure Active Directory
- SQL Server Management Studio

This implementation provides a foundation for hybrid identity management, enabling seamless integration between on-premises Active Directory and cloud services.

