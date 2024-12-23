
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
- SQL Server setup





- Basic configuration

### 2.2 Security Configuration 
- Authentication setup
- Database creation
- User permissions

## 3. Verification and Testing 
### 3.1 Identity Verification 
- Test user synchronization
- Verifty user access
- Document test results



