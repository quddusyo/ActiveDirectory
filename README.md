# ActiveDirectory
Project and markup notes for Active Directory

To reset password for user 'sophie', run command:
Set-ADAccountPassword sophie -Reset -NewPassword (Read-Host -AsSecureString -Prompt 'New Password') -Verbose

 force a password reset at the next logon for user 'sophie' with the following command:
 Set-ADUser -ChangePasswordAtLogon $true -Identity sophie -Verbose
