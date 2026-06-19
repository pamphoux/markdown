# Pilote ODBC pour HFSQL

## Introduction

##### <u>Windev Express</u>

- [ ] VM Windows 2025 Standard

PCSoft met à disposition des développeurs, une version de Windev nommée Express. Bien que très limitée ;  en plus de l'impossibilité d'ouvrir un projet crée depuis la version commerciale, ... *Chercher d'autres défauts L'import de données bloque au delà d'un certain nombre de colonnes (ou de clés)*

##### <u>HFSQL </u>

- [ ] Debian 13

- [ ] VM Windows 2025 Standard

HFSQL (anciennement HyperFile SQL) est un moteur de base de données développé par PC SOFT, intégré nativement à l'environnement de développement WinDev.  Bien que ses fondements techniques soient comparés à une ancienne base de données ISAM de type **<mark>dBASE</mark>**, nécessitant des opérations de maintenance comme le réindexage, il offre des fonctionnalités avancées telles que la gestion des transactions, l'intégrité référentielle et le **<mark>support SQL</mark>** conforme à la norme ANSI SQL-92

In my opinion, Hyperfile SQL should be used with small applications with a small amount of features and datas. 

###### dBASE (ISAM)

La chaine de connexion à l'instance en mode ISAM (Que je déconseille, sauf si vous aimez les ennuis) doit comporter le chemin vers le fichier de l'analyse (le fameux WDD). Cela signifie donc que vous devez installer Windev Express, car c'est depuis cet IDE que sont crées les WDD.

###### SQL (ANSI SQL-92)

La chaine de connexion à l'instance en mode SQL, n'a pas besoin du **WDD**.  Elle est du type : iodbctest "DSN=boss;UID=admin;PWD=Admin1234*;Database=linux"

##### <u> Driver ODBC</u>

- [ ] Debian 13

- [ ] VM Windows 2025 Standard

Archive une fois dézippée, un script installe et configure le pilote en 64 bit. Il est possible de configurer et tester les DSN en mode graphique avec iodbc.

Pas encore eu le temps de regarder pour Window$

## Fichiers nécessaires pour le deroulé des scénarios

Les versions des différents EXE PCSoft (Windev, HFSQL, ODBC) doivent **IMPÉRATIVEMENT** avoir les numéros de version interne suivant si on utilise la release 28 :
![](https://drive.google.com/drive-viewer/AKGpihabv9Oe-g5aFYfYvrusRr3Lp85-DLCM1WeGCQ3hI7Y5mmrrz08l_SXFdYXRCCPKpU1KizVFCm92T4RdNIgqIvro8OF9-kc5FYQ=w1920-h946-rw-v1?auditContext=forDisplay)
![](https://85.215.170.153/img/md/win.jpg)
![](https://85.215.170.153/img/md/win.jpg)

Windev Express <mark>28</mark> Version Interne [90F280094e](https://pcsoft.fr/windev/WD-Express.htm) **WDEXPRESSFR.exe**

HFSQL Client/serveur (Windows et Linux) Version interne :
[90F280094e](https://package.windev.com/pack/wx28/wx28_95g/fr/commun/HFSQL28INSTALL094e.exe) **HFSQL<mark>28</mark>INSTALL<mark>094</mark>e.exe**

Driver ODBC pour HFSQL (Windows) Version Interne [01F280095g](https://package.windev.com/pack/wx28/wx28_95g/fr/commun/ODBC28PACK095g.exe) **ODBC<mark>28</mark>PACK<mark>095g</mark>.exe**

Driver ODBC pour HFSQL (Linux) Version Interne [01F280095g](https://package.windev.com/pack/wx28/wx28_95g/fr/commun/ODBC28LINUX64PACK095g.zip) **ODBC<mark>28</mark>LINUX64PACK<mark>095g</mark>.zipODBC<mark>28</mark>LINUX64PACK<mark>095g</mark>.zip**

### Installation du pilote ODBC pour Linux

[Link](https://doc.pcsoft.fr/fr-FR/?9000160)

Le driver ODBC pour HFSQL Client/Serveur permet d'accéder à une base de données HFSQL Client/Serveur depuis un logiciel de base de données externe, gérant les accès par ODBC.

Le driver est disponible en lecture et en 
écriture. Une application écrite dans un langage tiers peut lire et 
écrire dans des fichiers de données HFSQL.

Le driver ODBC pour HFSQL Classic et HFSQL
 Client/Serveur est un driver ODBC de niveau 3.

[Lien vers la Doc](https://doc.pcsoft.fr/fr-FR/search2.awp?origin=browse&cat=gestion-bases-donnees,367)

```
unzip ~/Downloads/ODBCLINUX64PACK095g.zip
cd ~/Downloads/ODBC28LINUX64PACK095g
chmod +x install.sh
sudo ./install.sh
```

`HFSQL ODBC Driver (64bit) Installation Script
Copyright PCSOFT.`

`Starting install for HFSQL ODBC Driver (64bit)`

`Checking for driver ...
Verifying if iODBC is present ...
Checking for previous installation ...
ERROR : HFSQL ODBC Driver has already been installed. Please remove any references to it in /etc/odbcinst.ini before reinstalling.`

```
sudo vi /etc/odbcinst.ini
```

`[ODBC Drivers]`

`HFSQL = Installed`

`[HFSQL]
Description = HFSQL iODBC Driver
Driver = /home/pierre/Workspace/PCSoft/ODBC/wxpack_iodbclinux313016/wd310hfo64.so`

### Installation des paquets complémentaires

[Link](https://doc.pcsoft.fr/fr-FR/?9000160)

#### Installation du manager [iODBC](https://www.iodbc.org/dataspace/doc/iodbc/wiki/iodbcWiki/WelcomeVisitors)

```
sudo apt update && sudo apt upgrade -y
sudo apt install libiodbc2-dev  
sudo apt install iodbc
sudo apt install libiodbc2 
```

#### Programmes Installés

###### iodbctest

```
iodbctest --help
```

`iODBC Demonstration program
This program shows an interactive SQL processor`

`Usage:
 iodbctest ["DSN=xxxx;UID=xxxx;PWD=xxxx"]`

###### iodbcadm-gtk

![](/home/pierre/.config/marktext/images/2026-06-13-18-41-46-image.png)

# Premier Scénario

Poste Serveur Windows 2025 : Windev Express 28 / HFSQL 28

Poste client Debian 13 : ODBC 28

## 1. Restauration de la BDD SYME05

[Procédure de restauration BDD](backup.md)

## 2. Création / Test de la DNS

##### Mode Manuel

vi ~/.odbc.ini

En mode local

```
[ODBC Data Sources]
BOSS = HFSQL
[BOSS]
Server Name = localhost
Server Port = 4900
Database = SYME05
UID = admin
PWD = Admin1234*
```

iodbctest "DSN=BOSS;UID=admin;PWD=Admin1234*;Database=SYME05"

En mode Distant

```
[ODBC Data Sources]
SRVHFSQL = HFSQL
[SRVHFSQL]
Server Name = 192.168.100.104
Server Port = 4900
Database = SYME05
UID = admin
PWD = Admin1234*
```

iodbctest "DSN=SRVHFSQL;UID=admin;PWD=Admin1234*;Database=SYME05"

```
iODBC Demonstration program
This program shows an interactive SQL processor
Driver Manager: 03.52.0812.0326
Driver: 31.00.24100 (wd310hfo64.so)
SQL>
```

select count(*) from dd_dossier;

```
Expr1
9677
result set 1 returned 1 rows.
```

##### En Mode Graphique

![](/home/pierre/.config/marktext/images/2026-06-13-18-49-51-image.png)

![](/home/pierre/.config/marktext/images/2026-06-13-18-45-33-image.png)

![](/home/pierre/.config/marktext/images/2026-06-13-18-46-34-image.png) 

## iodbctest

### Connexion

iODBC Demonstration program
This program shows an interactive SQL processor
Driver Manager: 03.52.0812.0326
Driver: 31.00.24100 (wd310hfo64.so)
Usage: iodbctest ["DSN=xxxx;UID=xxxx;PWD=xxxx"]

Exemple
iodbctest "DSN=SRVHFSQL;UID=admin;PWD=Admin1234*"
iodbctest "DSN=SRVHFSQL;UID=admin;PWD=Admin1234*;Database=syme05"

### Select

## 3. Tables

Création

iodbctest "DSN=BOSS;UID=admin;PWD=Admin1234*;Database=sdea10"

CREATE TABLE dd_dossier (id_dossier INTEGER PRIMARY KEY AUTO_INCREMENT,lb_designation VARCHAR(50) NOT NULL) GO

https://package.windev.com/pack/wx27/wx27_104h/fr/commun/HFSQL27INSTALL104.exe
https://package.windev.com/pack/wx27/wx27_104h/fr/commun/ODBC27PACK104h.exe
https://package.windev.com/pack/
https://package.windev.com/pack/wx28/wx28_95g/fr/commun/HFSQL28INSTALL094e.exe
