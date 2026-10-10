# Configuration Serveur de Production

> [!WARNING]
>
> Ne pas activer le réseau avant ces étapes
>

## Hors Connexion

### Copie des Utilitaires 

VM éteinte monter la partition Données de base Microsoft, copier La dernière version de :

- [ ] Power Shell
- [ ] Notepad++
- [ ] 7zip
- [ ] Winaero Portable
  Disable
- [ ] Updates
- [ ] Telemetry
- [ ] Shortcuts arrows
- [ ] Enable User Auto Logon Checkbox (netplwiz)
- [ ] Disable Background Apps

- [ ] Winscript Portable
  Supprimer Accueil et Galerie de l'Explorateur de Fichiers
  Performances Tous sauf :
  Limiter CPU Defender

### Windows Defender

```powershell
Remove-WindowsFeature Windows-Defender
Computer-Restart
```

### NetBIoS

Nom : SRVBDD...
WorkGroup : MERINDOLE
Description : SQL Server Express SAGE 100

## Réseau Actif

> [!IMPORTANT]
>
> Activer le réseau, en installant le pilote virtio

### Activer Windows

```powershell
irm https://get.activated.win | iex
```

### OpenSSH
```powershell
Get-WindowsCapability -Online | ? Name -like 'OpenSSH*'

Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Remove-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
Remove-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

Get-Service sshd
Status   Name               DisplayName
------   ----               -----------
Stopped  sshd               OpenSSH SSH Server

Set-Service sshd -StartupType Automatic
Get-Service sshd | Set-Service -StartupType Automatic
Get-Service sshd | Start-Service | Set-Service -StartupType Automatic



Get-Service ssh-agent | Set-Service -StartupType Automatic | Start-Service

Start-Service ssh-agent
Get-Service ssh-agent
Stop-Service ssh-agent
```


### POWERSHELL

Installer toujours la dernière version
[Source](https://github.com/powershell/powershell)

Définir PWSH comme shell par défaut pour OpenSSH
```powershell
$NewItemPropertyParams = @{
    Path         = "HKLM:\SOFTWARE\OpenSSH"
    Name         = "DefaultShell"
    Value        = "C:\Program Files\PowerShell\7\pwsh.exe"
    PropertyType = "String"
    Force        = $true
}

New-ItemProperty @NewItemPropertyParams
```

```powershell
DefaultShell : C:\Program Files\PowerShell\7\pwsh.exe
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\OpenSSH
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE
PSChildName  : OpenSSH
PSDrive      : HKLM
PSProvider   : Microsoft.PowerShell.Core\Registry
```

### Test connection Hôte

ssh pierre@85.215.170.153 -p 51200 -i "C:\Users\Administrateur\.ssh\pierre+2027@lamerindole"

ssh -R 63390:localhost:3389 pierre@85.215.170.153 -p 51200 -i "C:\Users\Administrateur\.ssh\pierre+2027@lamerindole"

### Chocolatey

```powershell
cd C:\Temp
Get-ExecutionPolicy
# Must return : ByPass
# If not , run
# Set-ExecutionPolicy Bypass -Scope Process -Force

Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

refreshenv

choco -v
2.7.4
```

```powershell
choco install 7zip notepadplusplus winscp mobaxterm -y
```

```powershell
choco install googlechrome firefox -y
```

```powershell
choco install git vscode powershell-core -y
```

```powershell
choco install curl putty sysinternals -y
```



### NSSM

mkdir C:\nssm
cd C:\nssm\win64

.\nssm.exe install wrdp.ssh.tunnel.service





@ECHO OFF
cd  C:\Users\Administrateur\.ssh\
ssh -R 49907:localhost:49907 IONOS
::ssh -R 49907:localhost:49907 -p 51200 pierre@85.215.170.153 -i C:\Users\Administrateur\.ssh\pierre+jan2026@boss
::ssh IONOS

@ECHO OFF
REM Pour gérer un service:
::nssm start appli.mobile.ssh.tunnel
REM nssm stop appli.mobile.ssh.tunnel
nssm restart appli.mobile.ssh.tunnel







### Winaero Tweaker

