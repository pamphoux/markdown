## Pilote ODBC pour linux 

libiodbc2 entre en conflit avec unixodbc
```sudo apt remove --purge unixodbc-dev```
Le paquet (XXX) disponible sur apt pour Debian 13, ne fonctionne pas. Il faut passer par DPKG. Lien vers la [3.52.9-3](http://ftp.de.debian.org/debian/pool/main/libi/libiodbc2/iodbc_3.52.9-3_amd64.deb)
Ce qui correspond à une Debian 12

Le paquet iodbc n'est plus disponible sur les repo Trixie. Lien [libiodbc2_3.52.9-3_amd64.deb](http://ftp.de.debian.org/debian/pool/main/libi/libiodbc2/libiodbc2_3.52.9-3_amd64.deb)

cd ~/Workspace/PCSoft/ODBC/linux/Debian12
sudo dpkg -i iodbc_3.52.9-3_amd64.deb
sudo dpkg -i libiodbc2_3.52.9-3_amd64.deb

Les programmes suivants sont installés:
- odbcinst : 
- iodbctest : interface CLI de requêtes (SELECT, INSERT, UPDATE, DELETE, etc ...)
- iodbcadm-gtk (outil de config graphique de la DNS)

### Les fichiers de config ODBC

odbc.ini : Fichier de config des DSN ODBC
odbcinst.ini : Fichier de config du pilote ODBC

#### Pour tous les users
cat /etc/odbc.ini
``
cat /etc/odbcinst.ini
[ODBC Drivers]
HFSQL = Installed

[HFSQL]
Description = HFSQL ODBC Driver
Driver = /home/pierre/Workspace/PCSoft/ODBC/ODBC28LINUX64PACK095g/wd280hfo64.so

#### Par user
cat ~/.odbc.ini
```
[ODBC Data Sources]
WIN2025 = HFSQL
[WIN2025]
Driver      = /home/pierre/Workspace/PCSoft/ODBC/ODBC28LINUX64PACK095g/wd280hfo64.so
Database    = syme05
PWD         = abder
Server Name = localhost
Server Port = 49000
UID         = abder
```
cat ~/.odbcinst.ini

