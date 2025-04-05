
# Scripts
## About
Kumpulan script yang digunakan sebagai contoh pada SecuringSME

## List account local pada windows

### list account local dengan PowerShell
PowerShell:
```PowerShell
Get-LocalUser
```

### list account local dengan Command Prompt
Command:
```cmd
net user
```

Command:
```cmd
wmic useraccount get Name,Status,Disabled,AccountType,Lockout,PasswordRequired,PasswordChangeable,SID
```

## List user yang tidak logon lebih dari 30d

referensi: [microsoft - inactive user](https://learn.microsoft.com/en-us/services-hub/unified/health/remediation-steps-ad/regularly-check-for-and-remove-inactive-user-accounts-in-active-directory)

PowerShell:
```PowerShell
$d = [DateTime]::Today.AddDays(-180)

Get-ADUser -Filter '(PasswordLastSet -lt $d) -or (LastLogonTimestamp -lt $d)' -Properties PasswordLastSet,LastLogonTimestamp | ft Name,PasswordLastSet,@{N="LastLogonTimestamp";E={[datetime]::FromFileTime($_.LastLogonTimestamp)}}
```

## ACL pada folder/ file security windows

referensi: [microsoft](https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-acl?view=powershell-7.5)

### mendapatkan setting ACL sebuah folder
PowerShell:
```PowerShell
Get-Acl C:\Windows
```

### copy setting ACL dari file lain
PowerShell:
```PowerShell
Get-Acl -Path "C:\Dog.txt" | Set-Acl -Path "C:\Cat.txt"
```