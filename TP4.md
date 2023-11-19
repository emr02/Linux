 # TP 4 - Utilisateurs, groupes et permissions

<h1>Exercice 1. Commandes de base</h1>

<ol>
<li><h3>Créez un alias “maj” permettant de télécharger la dernière liste des paquets disponibles, puis de mettre à jour les paquets installés. Où faut-il enregistrer cet alias pour qu’il ne soit pas perdu au prochain
redémarrage ?<h3></li>
<li><h3>Mettez à jour votre système en utilisant l’alias créé précédemment. Pourquoi cette commande s’exécute sans problème, alors qu’elle est définie dans votre dossier personnel (“home”), et que celui-ci ne figure pas dans le PATH ?<h3></li

*En allant dans le fichier .bashrc, situé dans /home/julien pour le user julien, on remarque le script en bash suivant:*

```bash
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
. ~/.bash_aliases
fi
```
*Nous avons donc créé le fichier `.bash_aliases` , dont le contenu est ci-dessous :*

```bash
alias maj='apt upgrade'
```
*Nous devons par la suite nous déconnecter de la session afin que notre fichier soit pris en compte mais non pas redémarré la machine sinon nous perdons tout :
Nous avons entré les commandes suivantes :
`sudo su` puis `maj` et nous avons remarqué que notre alias est bien pris en compte car le système va lancer une recherche de mise à jour*

<li><h3>Combien de paquets sont disponibles en téléchargement sur les dépôts Ubuntu ?</h3></li>


```bash
La commande pour lister le nombre de paquets disponible en téléchargement est la suivante :
apt list | wc -l
Le résultat de la commande est : 61768 paquets disponibles au téléchargement
```

<li><h3>Utilisez les commandes dpkg et apt pour compter de deux manières différentes le nombre de total de paquets installés sur la machine (ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage ? Pourquoi ne peut-on pas utiliser directement le fichier dpkg.log ?</h3></li>

```bash
La commande est la suivante pour dpkg: dpkg -l | wc -l
Le résultat est : 563
le wc -l permet de compter le nombre de ligne et comme un paquet est équivalent à une ligne
on trouve le bon nombre de paquets
La commande pour lister les paquets installés avec apt et les compter est la suivante :
apt list -- intstalled | wc -l
La petite différence de comptage entre apt et dpkg s'explique il y a des lignes warning que le wc -l compte
```

<li><h3>Utilisez le fichier /var/log/dpkg.log pour obtenir les 5 derniers paquets installés sur votre machine.</h3></li>

```bash
$ grep install /var/log/dpkg.log | tail -n 5

Le résultat de la commande est le suivant :

2020-04-01 08:52:09 status installed linux-image-5.3.0-45-generic:amd64 5.3.0-45.37
2020-04-01 08:51:31 status installed initramfs-tools:all 0.133ubuntu10 2020-04-01 08:50:51
status installed mime-support:all 3.63ubuntu1 2020-04-01 08:50:51 status installed libcbin:
amd64 2.30-0ubuntu2.1 2020-04-01 08:50:51 status installed install-info:amd64 6.6.0.dfsg.1-
2ubuntu2
```

<li><h3>Listez les derniers paquets qui ont été installés explicitement avec la commande apt install</h3></li>

<li><h3>A quoi servent les paquets glances, tldr et hollywood ? Installez-les et testez-les.</h3></li>

<li><h3>Quels paquets proposent de jouer au sudoku ?</h3></li>

<b>N’installez pas le paquet gnome-sudoku ou ksudoku sous peine de devoir probablement réinstallervotre VM</b>

</ol>
