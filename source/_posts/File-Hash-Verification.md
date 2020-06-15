---
title: File Hash Verification
date: 2020-06-10 20:30:19
tags:
  - win10
  - Right click on the menu
  - PowerShell
categories:
  - Regedit
---

**摘要：**利用PowerShell以及win10自带Hash工具，对文件进行hash验证，添加到右击菜单中。

<!---more--->

**思路：**直接利用`.reg`文件的方式添加注册表信息。

**文件内容（添加）：**

```
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\*\shell\File Hash Verification]

"SubCommands"="MACTripleDES;MD5;RIPEMD160;SHA1;SHA256;SHA384;SHA512"

"MUIVerb"="File Hash Verification"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\MACTripleDES]

@="MACTripleDES"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\MACTripleDES\command]

@="PowerShell Get-FileHash -Algorithm MACTripleDES \\\"%1\\\" | format-list;“按任意键退出...”;[Console]::Readkey() | Out-Null;exit"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\MD5]

@="MD5"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\MD5\command]

@="PowerShell Get-FileHash -Algorithm MD5 \\\"%1\\\" | format-list;“按任意键退出...”;[Console]::Readkey() | Out-Null;exit"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\RIPEMD160]

@="RIPEMD160"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\RIPEMD160\command]

@="PowerShell Get-FileHash -Algorithm RIPEMD160 \\\"%1\\\" | format-list;“按任意键退出...”;[Console]::Readkey() | Out-Null;exit"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\SHA1]

@="SHA1"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\SHA1\command]

@="PowerShell Get-FileHash -Algorithm SHA1 \\\"%1\\\" | format-list;“按任意键退出...”;[Console]::Readkey() | Out-Null;exit"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\SHA256]

@="SHA256"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\SHA256\command]

@="PowerShell Get-FileHash -Algorithm SHA256 \\\"%1\\\" | format-list;“按任意键退出...”;[Console]::Readkey() | Out-Null;exit"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\SHA384]

@="SHA384"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\SHA384\command]

@="PowerShell Get-FileHash -Algorithm SHA384 \\\"%1\\\" | format-list;“按任意键退出...”;[Console]::Readkey() | Out-Null;exit"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\SHA512]

@="SHA512"

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\SHA512\command]

@="PowerShell Get-FileHash -Algorithm SHA512 \\\"%1\\\" | format-list;“按任意键退出...”;[Console]::Readkey() | Out-Null;exit"
```

**删除**只需要在`[]`中最前面加`-`, 例如：

```
# 以MD5删除为例：

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\MD5]

@="MD5"

[-HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\CommandStore\shell\MD5\command]

@="PowerShell Get-FileHash -Algorithm MD5 \\\"%1\\\" | format-list;“按任意键退出...”;[Console]::Readkey() | Out-Null;exit"
```

