# NSSM - Windows Service Manager

## Installation
Extraire l'archive dans C:\'Program Files'\nssm

`mkdir 'C:\Program Files\nssm'`
`cd C:\Sources`
`7z.exe t nssm-2.24-103-gdee49fc.zip`
`7z.exe x nssm-2.24-103-gdee49fc.zip -o'C:\Program Files\nssm'`
cd 'C:\Program Files\nssm'`
mkdir 'C:\Program Files\nssm\log'
mkdir 'C:\Program Files\nssm\log\stderr'
mkdir 'C:\Program Files\nssm\log\stdout'
New-Item 'C:\Program Files\nssm\log\stderr\error.log' -type file
New-Item 'C:\Program Files\nssm\log\stdout\access.log' -type file

## Modification du PATH
####Ajout EXE amd64

setx PATH "$env:path;'C:\Program Files\nssm\win64" -m

## Service

#### Ajouter un service
