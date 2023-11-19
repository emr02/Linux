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

<li><h3>Utilisez les commandes dpkg et apt pour compter de deux manières différentes le nombre de total de paquets installés sur la machine (ne pas hésiter à consulter le manuel !). Comment explique-t-on la (petite) différence de comptage ? Pourquoi ne peut-on pas utiliser directement le fichier dpkg.log ?</h3></li>

<li><h3>Utilisez le fichier /var/log/dpkg.log pour obtenir les 5 derniers paquets installés sur votre machine.</h3></li>

<li><h3>Listez les derniers paquets qui ont été installés explicitement avec la commande apt install</h3></li>

<li><h3>A quoi servent les paquets glances, tldr et hollywood ? Installez-les et testez-les.</h3></li>

<li><h3>Quels paquets proposent de jouer au sudoku ?</h3></li>

<b>N’installez pas le paquet gnome-sudoku ou ksudoku sous peine de devoir probablement réinstallervotre VM</b>

</ol>
