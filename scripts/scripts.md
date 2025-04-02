
# Scripts
Kumpulan script yang digunakan sebagai contoh pada SecuringSME

## List account local pada windows

PowerShell:
```PowerShell
Get-LocalUser
```

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