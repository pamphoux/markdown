# SQL Express 2025

[Source](https://download.microsoft.com/download/ffd82b4c-9955-47c0-8efe-6290f7795cf6/SQL2025-SSEI-Expr.exe)

## Installation

```powershell
cd C:\Temp
Set-ExecutionPolicy Bypass -Scope Process -Force
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org'))
refreshenv
choco -v
```

Installation silencieuse
