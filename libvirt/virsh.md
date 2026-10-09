# Administration du VPS - Virsh

# [A] Présentation

[Doc](https://blog.stephane-robert.info/docs/virtualiser/type1/kvm/virsh-commandes/)

> [!NOTE]
>
> virsh est l'outil CLI standard pour piloter libvirt, il gère les
> VMs (domains), les réseaux virtuels et le stockage.

## [A.1] Modèle

virsh gère 3 types de ressources : 

- domains (VMs)
- networks
- storage pools

### [A.1.a] Domain

Une VM dans le vocabulaire libvirt

### [A.1.b] Network

### [A.1.c] Pools

## [A.2] Vérification du service

> [!IMPORTANT]
>
> Toujours vérifier l'état avant d'agir.
> Ne pas démarrer une VM déjà running.

### [A.2.a] Résumé

```sh
sudo virsh list --all

 ID   Nom       État
-----------------------
 -    SAGE100   fermé
```

### [A.2.b] Informations complètes

```sh
sudo virsh dominfo SAGE100

ID :           -
Nom :          SAGE100
UUID :         07a45d40-cf67-4cc1-898e-d274ad181b2c
Type de SE :   hvm
État :        fermé
CPU :          2
Mémoire Max : 2097152 KiB
Mémoire utilisée : 2097152 KiB
Persistant :    oui
Démarrage automatique : désactiver
Autostart Once: désactiver
Sauvegarde gérée : non
Modèle de sécurité : apparmor
Sécurité DOI : 0
```

## [B] Commandes

### [B.1] Démarrer

```sh
sudo virsh start SAGE100

Domaine 'SAGE100' démarré

 ID   Nom       État
--------------------------------------
 6    SAGE100   en cours d’exécution
```
