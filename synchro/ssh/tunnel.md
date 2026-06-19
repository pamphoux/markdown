## Tunnel

#### Important !!!
Le port sur lequel écoute le serveur du moteur de BDD HFSQL doit être changé. Le port par défaut est le TCP 4900, aujourd'hui, pour des raisons de sécurité, les FAI (free, orange, ...) et les providers VPS (Ionos, Contabo, ...) ont fermé les accès sur les classes suivantes 
- Well Known Port (0-1023)
- Registered Port (1024-49151)

Le NAT est permis Seulement pour la classe Dynamic Port (49152-65535)
Dans le HFSQL Control Center, il faut faire écouter le service sur le port 54900 (par exemple)
``ssh -R 59400:localhost:59400 pierre@85.215.170.153 -p 51200 -i ~/.ssh/pierre+jan2026@boss``
