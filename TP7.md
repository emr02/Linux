# TP 7 - ANSIBLE


1. fichier - outils - network manager - ajouter nat neworks
   pour les machines virtuelles, configuration - reseaux - activer adapter2 - selectionner reseaux nat
   
```
$ nano /etc/netplan/50-cloud-init.yaml

network:
   ethernets:
       enp0s3:
           dhcp4: true  (il faut enp0s3 car il sert à se connecter à internet)
       enps08:
           dhcp4: false
           addresses:
           − 192.168.122.10/24
   version: 2

puis faire sudo netplan apply pour sauvergarder config

On peut cloner la machine et config .11 .12 avec netplan

```
2. Résolution des noms
Pour que le node manager communique avec les serveurs par un nom plutôt que par une adresse IP, il fau-
drait configurer un serveur DNS comme nous l’avons fait dans le TP 6. Cependant, il s’agit d’un processus
assez long, et l’objet du présent TP étant Ansible, nous allons utiliser une méthode plus rapide.
Ajoutez les deux entrées suivantes au fichier /etc/hosts :
- 192.168.122.11 http1
- 192.168.122.12 bdd1
 Le fichier /etc/hosts est en quelques sortes l’ancêtre des serveurs DNS, à une époque où la résolution
de noms se faisait à la main... Il est toujours utilisé, et est consulté avant de faire appel à un serveur DNS.
Vous pouvez à présent créer deux clones de cette machine

3. Test de la configuration
   - modifiez l’adresse IP et le fichier hosts de chaque serveur ;
   - renommez le serveur web en http1 et le serveur de base de données en bdd1 (reconnectez-vous
pour que les modifications apparaissent) ;
   - Vérifiez que les trois machines arrivent à communiquer entre elles par leur nom et à aller
sur Internet.

4. Installer Ansible

```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo add-apt-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```

on vérifie avec ls -l a* que les fichiers ansible sont présent dans /usr/bin

5. creer utilsateur user-ansbile et l'ajouter à sudo

```
sudo add user-ansible
```
6. Depuis le node manager, connectez-vous en SSH au compte admin de chaque node. Ceci permet d’enregistrer
sur le node manager l’empreinte SSH de chaque node, une étape indispensable pour pouvoir utiliser Ansible

```
$ su user-ansible  (changer utilisateur)
$ ssh tp@http1
$ exit
$ ssh tp@bdd1
$ exit

sudo apt install sshpass (car pas installé)

```
7. Créez un dossier ansible dans /home/user-ansible sur le node manager, et créez votre fichier
d’inventaire nommé inventaire.ini dans ce dossier. Le node http1 sera placé dans un groupe
nommé apache et le node bdd1 sera placé dans un groupe nommé db.

```
Dans le fichier inventaire.ini :

[apache]
http1

[db]
bdd1
```
8. Commencez par lancer un ping sur les nodes à l’aide d’Ansible, avec la commande
suivante :
```
remplacer tp par le nom de votre utilisateur capable d'executer sudo

$ ansible -i inventaire.ini -m ping all -u tp --ask-pass

Si tout va bien, vous devriez obtenir le résultat suivant :
         http1 | SUCCESS => {
            "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
            },
            "changed": false,
            "ping": "pong"
         }
         bdd1 | SUCCESS => {
            "ansible_facts": {
            "discovered_interpreter_python": "/usr/bin/python3"
            },
            "changed": false,
            "ping": "pong"
         }

```
9. Installez sur le node-manager le paquet python3-passlib, puis générez la version chiffrée
du mot de passe du compte :

```
$ ansible localhost -i inventaire.ini -m debug -a "msg={{ 'mot de passe' |
password_hash('sha512', 'secretsalt') }}"

```
10. Utilisez le mot de passé chiffré de la question précédente et le module user pour
créer le nouvel utilisateur user-ansible sur les nodes (cherchez dans la documentation pour
en savoir plus sur ce module !).
 Les seuls arguments nécessaires du module user sont ici le nom et le mot de passe de notre nouvel
utilisateur.
Si tout s’est bien passé, vous devriez obtenir un message de couleur orange, qui indique qu’une modification
a eu lieu sur les nodes. Notez également la ligne "password": "NOT_LOGGING_PASSWORD" : le mot de passe
de l’utilisateur ne figurera pas dans les logs sur le node

voir question 9 et mettre directement dans le code de la question 9. avec user à la place de debug et -a ... les arguments
