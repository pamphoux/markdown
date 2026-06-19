# NSSM - Windows Service Manager

## Installation
Extraire l'archive dans C:\'Program Files'\nssm

`mkdir 'C:\Program Files\nssm'
cd C:\Sources
7z.exe t nssm-2.24-103-gdee49fc.zip
7z.exe x nssm-2.24-103-gdee49fc.zip -o'C:\Program Files\nssm'
cd 'C:\Program Files\nssm'
mkdir 'C:\Program Files\nssm\log'
mkdir 'C:\Program Files\nssm\log\stderr'
mkdir 'C:\Program Files\nssm\log\stdout'
New-Item 'C:\Program Files\nssm\log\stderr\error.log' -type file
New-Item 'C:\Program Files\nssm\log\stdout\access.log' -type file`

## Modification du PATH
####Ajout EXE amd64
setx PATH "$env:path;'C:\Program Files\nssm\win64" -m

## Service
### nssm.exe --help

### Ajouter un service
nssm.exe install nom_du_service

### Exemple : Connexion RDP à la VM SRVHFSQL via un Tunnel SSH
Le service libvirtd (QEMU/KVM) propose 2 modes de connexion réseau:
- En mode User session : Tout est fermé, l'accès à Internet est ok, par contre la VM ne voit qu'une seule machine sur le réseau; l'hôte. Dans cette configuration, l'hôte à pour IP : 10.0.2.2 et le guest 10.0.2.12
- En mode système : La VM est reachable depuis l'extérieur, mais pas directement. Le fournisseur VPS n'autorise pas la connexion directe sur une VM. Il existe un moyen simple de pouvoir atteindre une VM sur un VPS.

 Créer un tunnel SSH avec RemoteForward.
 Créer une régle de redirection NAT.
