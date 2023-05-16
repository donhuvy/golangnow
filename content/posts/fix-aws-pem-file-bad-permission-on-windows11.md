---
categories:
  - AWS
tags:
  - AWS
  - EC2
title: "Fix AWS EC2 pem file - Bad Permission on Windows 11"
date: 2023-05-01T09:02:29+07:00
draft: false
---

## Error bad permissions (too open)

```bash
Microsoft Windows [Version 10.0.22621.1555]
(c) Microsoft Corporation. All rights reserved.

D:\vydown>ssh -i "vydn_vmodev.pem" ec2-user@ec2-13-213-47-59.ap-southeast-1.compute.amazonaws.com
The authenticity of host 'ec2-13-213-47-59.ap-southeast-1.compute.amazonaws.com (13.213.47.59)' can't be established.
ED25519 key fingerprint is SHA256:qnjwvPDaiXvfpXOe7KStil6Jj/hL3joRbjvbzezy494.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'ec2-13-213-47-59.ap-southeast-1.compute.amazonaws.com' (ED25519) to the list of known hosts.Bad permissions. Try removing permissions for user: NT AUTHORITY\\Authenticated Users (S-1-5-11) on file D:/vydown/vydn_vmodev.pem.
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions for 'vydn_vmodev.pem' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "vydn_vmodev.pem": bad permissions
ec2-user@ec2-13-213-47-59.ap-southeast-1.compute.amazonaws.com: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).
```

![aws_pem_bad.png](/img/2023_05_01_aws_pem_bad_permission/aws_pem_bad.png)

## Solution 1: Use PowerShell (As administrator)

```bash
icacls.exe vydn_vmodev.pem /reset

icacls.exe vydn_vmodev.pem /inheritance:r
```

PowerShell run as administrator

```
Windows PowerShell
Copyright (C) Microsoft Corporation. All rights reserved.

Install the latest PowerShell for new features and improvements! https://aka.ms/PSWindows

PS C:\WINDOWS\system32> cd /d D:\vydown
Set-Location : A positional parameter cannot be found that accepts argument 'D:\vydown'.
At line:1 char:1
+ cd /d D:\vydown
+ ~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Set-Location], ParameterBindingException
    + FullyQualifiedErrorId : PositionalParameterNotFound,Microsoft.PowerShell.Commands.SetLocationCommand

PS C:\WINDOWS\system32> cd D:\vydown
PS D:\vydown> icacls.exe vydn_vmodev.pem /GRANT:R "$($env:USERNAME):(R)"
processed file: vydn_vmodev.pem
Successfully processed 1 files; Failed processing 0 files
PS D:\vydown>
```


```bash
icacls.exe vydn_vmodev.pem /reset

icacls.exe vydn_vmodev.pem /grant:r "$($env:username):(r)"

icacls.exe vydn_vmodev.pem /inheritance:r

icacls.exe vydn_vmodev.pem /GRANT:R "$($env:USERNAME):(R)"
```

![aws_pem_power_shell.png](/img/2023_05_01_aws_pem_bad_permission/aws_pem_power_shell.png)

## Solution 2: Use Windows Explorer GUI, set permission Read

![gui_pem.png](/img/2023_05_01_aws_pem_bad_permission/gui_pem.png)

![gui_pem.png](/img/2023_05_01_aws_pem_bad_permission/aws_connect_success.png)

See https://stackoverflow.com/a/43317244/3728901

https://gist.github.com/jaskiratr/cfacb332bfdff2f63f535db7efb6df93
