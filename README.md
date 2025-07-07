
# 🛡️ ActiveDirectory PowerShell Utilities

A collection of **PowerShell commands** for managing **Active Directory (AD)** in enterprise environments.  
Ideal for system administrators, IT security engineers, and help desk technicians.

---

## ⚠️ Notice

These PowerShell commands are context-dependent and should be executed based on your organization’s specific policies and access controls.
🛡️ Use with caution. Always validate changes in a test environment before applying them to production.

---

##  Purpose

This project serves as a quick reference for ActiveDirectory toolbox for roles related to:

- Managing user accounts
- Resetting and enforcing password policies
- Working with groups
- Exporting and filtering AD data
- Diagnosing account issues

---

##  Password Management

###  Reset a user's password
```powershell
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose
```

###  Force password change on next logon
```powershell
Set-ADUser -Identity sophie -ChangePasswordAtLogon $true -Verbose
```

###  Unlock a user account
```powershell
Unlock-ADAccount -Identity sophie -Verbose
```

---

##  User Account Management

###  Create a new user
```powershell
New-ADUser -Name "Sophie Adams" -SamAccountName sophie -UserPrincipalName sophie@domain.com `
  -Path "OU=Employees,DC=domain,DC=com" -AccountPassword (Read-Host -AsSecureString "Set Password") `
  -Enabled $true -Verbose
```

### Disable a user account 
* Disabling a user account in the workplace is a common security and administrative action. It's typically done to temporarily or permanently block access to systems without deleting the account or its data. *
Common Reasons: Employee Termination or Resignation, Extended Leave (e.g. Maternity, Sabbatical), Security Breach or Compromise, Policy Violations or Internal Investigations, Unused/Inactive Accounts, License or Budget Constraints
```powershell
Disable-ADAccount -Identity sophie
```
##  Best Practices
- Always log who/why/when accounts are disabled (for audit)
- Use automation (PowerShell, Azure AD policies, etc.)
- Combine with disabling mailbox access or remote login if needed
- Never delete accounts immediately (retain for logs, emails, etc.)

### Enable a user account
*Enabling an account in Active Directory (AD) is often a necessary step when you're restoring access to a user who previously had their account disabled — whether for security, HR, or administrative reasons.*
```powershell
Enable-ADAccount -Identity sophie
```

---

##  Group Management

### ➕ Add user to a group
```powershell
Add-ADGroupMember -Identity "HR Department" -Members sophie
```

### ➖ Remove user from a group
```powershell
Remove-ADGroupMember -Identity "HR Department" -Members sophie -Confirm:$false
```

###  List group members
```powershell
Get-ADGroupMember -Identity "HR Department"
```

---

## 🔍 Search and Filter

###  Find users by name
```powershell
Get-ADUser -Filter {Name -like "*sophie*"} -Properties * | Format-Table Name, SamAccountName, Enabled
```

###  Find all disabled accounts
```powershell
Search-ADAccount -AccountDisabled -UsersOnly | Select Name, SamAccountName
```

### ⏳ Find users who haven’t logged in in 90+ days
```powershell
Search-ADAccount -AccountInactive -UsersOnly -TimeSpan 90.00:00:00 | Select Name, LastLogonDate
```

---

##  Exporting

###  Export all users in an OU to CSV
```powershell
Get-ADUser -Filter * -SearchBase "OU=Employees,DC=domain,DC=com" -Properties * |
  Select-Object Name, SamAccountName, Enabled, LastLogonDate |
  Export-Csv -Path "C:\ADUsers.csv" -NoTypeInformation
```

---

##  Account Diagnostics

###  Check if an account is locked
```powershell
Get-ADUser -Identity sophie -Properties LockedOut | Select-Object SamAccountName, LockedOut
```

###  Get full details of a user
```powershell
Get-ADUser -Identity sophie -Properties * | Format-List
```

---

##  Tips

- Use `-WhatIf` to simulate actions without applying them
- Use `Format-Table` or `Format-List` to improve output readability
- Always test scripts in a safe environment before using in production

---
