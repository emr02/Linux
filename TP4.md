 # TP 4 - Utilisateurs, groupes et permissions

<h1>Exercice 1. Commandes de base</h1>

<ol>
<li><h3>Créez un alias “maj” permettant de télécharger la dernière liste des paquets disponibles, puis de mettre à jour les paquets installés. Où faut-il enregistrer cet alias pour qu’il ne soit pas perdu au prochain
redémarrage ?<h3></li>

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

<li><h3>Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell
</h3></li>

</ol>
