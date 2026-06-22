# Pilote ODBC pour HFSQL Windows

## Introduction
iodbctest n'est pas disponible sur les plateformes Windows (Linux / Mac). Pour se connecter sur le moteur HFSQL depuis un OS MS, les alternatives sont :
- isql (ou iusql)
- osql
- odbc

Ces binaires sont également dispo pour Linux. Avant de configurer le connecteur ODBC sur Windows, on va d'abord les tester sur UNIX.

## Fichiers de configuration ODBC
### Normes à respecter
Les binaires isql, osql, ... cherchent les fichiers de config dans des endroits définis. Bien qu'est possible d'utiliser des chemins personnalisés, il est fortement recommandé de suivre les recommandations de la norme ODBC.
De la même manière, l'emplacement des pilotes obei aussi à des normes.


### odbc.ini
C'est le fichier de configuration du pilote. On y trouve les sections :

[ODBC Data Sources]
BOSS     = HFSQL

[BOSS]
Driver      = Driver=/usr/lib/x86_64-linux-gnu/odbc/wd280hfo64.so
Database    = syme05
PWD         = abder
Server Name = BOSS
Server Port = 4900
UID         = abder

### odbcinst.ini
C'est le fichier de configuration des DSN. On y trouve les sections :

[ODBC Drivers]
HFSQL=Installed

[HFSQL]
Description=HFSQL ODBC Driver
Driver=/usr/lib/x86_64-linux-gnu/odbc/wd280hfo64.so



