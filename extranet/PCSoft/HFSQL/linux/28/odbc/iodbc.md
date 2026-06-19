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
- iodbctest : interface CLI de requêtes (SELECT, INSERT, UPDATE, DELETE, etc ...)
- iodbcadm-gtk (outil de config graphique de la DNS)

### Les fichiers de config ODBC
#### Pour tous les users
- /etc/odbcinst.ini
- /etc/odbc.ini
