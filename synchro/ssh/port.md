## Forwarded Ports

### Les types
- Le port du Serveur à atteindre (SQL Server, HFSQL, ...). C'est la source de données
- Le port du firewall de IONOS
- Le port du client

### RemoteForward
Les ports suivants sont forwardés depuis la DSN
- SSH : 22
- Samba : 445
- RDP : 3389
- HFSQL : 4900
- SQL Server : 49xxx - 61xxx

Vers IONOS
- SSH : 50122
- Samba : 61445
- RDP : 53389
- HFSQL : 54900
- SQL Server : 49xxx - 61xxx

vi ~/.ssh/config
Host IONOS
    HostName 85.215.170.153
    User pierre
    Port 51200
    IdentityFile ~/.ssh/pierre+jan2026@boss
    RemoteForward 54900 localhost:4900
    RemoteForward 53389 localhost:3389
    RemoteForward 5999 localhost:5900
    RemoteForward 61445 localhost:445

Le tunnel en Remote

### LocalForward
