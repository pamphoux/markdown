## Tunnel

## Service
### nssm.exe --help

### Ajouter un service
nssm.exe install nom_du_service

### Exemple : Connexion RDP à la VM SRVHFSQL via un Tunnel SSH
Le service libvirtd (QEMU/KVM) propose 2 modes de connexion réseau:
- En mode User session : Tout est fermé, l'accès à Internet est ok, par contre la VM ne voit qu'une seule machine sur le réseau; l'hôte. Dans cette configuration, l'hôte à pour IP : 10.0.2.2 et le guest 10.0.2.12
- En mode système : La VM est reachable depuis l'extérieur, mais pas directement. Le fournisseur VPS n'autorise pas la connexion directe sur une VM. Il existe un moyen simple de pouvoir atteindre une VM sur un VPS.
