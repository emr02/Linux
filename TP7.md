# TP 7 - ANSIBLE


1. fichier - outils - network manager - ajouter nat neworks
   pour les machines virtuelles, configuration - reseaux - activer adapter2 - selectionner reseaux nat
   
```
>nano /etc/netplan/50-cloud-init.yaml

network:
   ethernets:
       enp0s3:
           dhcp4: true  (il faut enp0s3 car il sert à se connecter à internet)
       enps08:
           dhcp4: false
           addresses:
           − 192.168.122.10/24
   version: 2

puis faire sudo netplan try pour sauvergarder config

On peut cloner la machine et config .11 .12 avec netplan

```

2. Installer Ansible

```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```
