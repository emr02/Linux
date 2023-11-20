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

*Cette commande va lister les 5 derniers paquets qui ont été installés, ce sont bien les 5 derniers car ils sont listés dans ce fichier : dpkg.log*

<li><h3>Listez les derniers paquets qui ont été installés explicitement avec la commande apt install</h3></li>

```bash
[root]@ubuntu-/home/julien: grep "apt install" /var/log/apt/history.log
Commandline: apt install ledernierpaquetsinstallé
```
<li><h3>A quoi servent les paquets glances, tldr et hollywood ? Installez-les et testez-les.</h3></li>

<li><h3>Quels paquets proposent de jouer au sudoku ?</h3></li>

```bash
apt search sudoku

Ci-dessus vont donc être listés tous les paquets utiles pour jouer au sudoku
```

<b>N’installez pas le paquet gnome-sudoku ou ksudoku sous peine de devoir probablement réinstallervotre VM</b>

</ol>

<h1>Exercice 2</h1>

<ol>
 
<li><h3>A l’aide des commandes which -a et dpkg, identifiez de quel paquet provient la commande ls ? Comment obtenir cette information en une seule commande, pour n’importe quel programme ? Utilisez la réponse à cette question pour écrire un script appelé origine-commande (sans l’extension .sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.<h3></li>

*La commande ls est installée à partir du paquet suivant :*
```bash
which -a ls | tail -n 1 | xargs dpkg -S
coreutils: /bin/ls
```

*script origine-commande*

```bash
if [ -z "$1" ]; then
echo "bad argument, donnez un nom de commande ."
else
which -a "$1" | tail -n 1 | xargs dpkg -S
fi
```
*test du script et il fonctionne !*

```bash
./origine-commande mkdir
coreutils: /bin/mkdir
```
</ol>

<h1>Exercice 3</h1>

<ol>
 
<li><h3>Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package spécifié dans cette commande.<h3></li>
 
```bash
(dpkg -l "fortunes" | grep "^i") && echo "installé" || echo "non installé"
```
*Résultat de la commande :*

```bash
ii fortunes 1:1.99.1-7build1 all Data files containing fortune cookies installé
```

*Autre exemple avec le paquet openssh-server :*
```bash
(dpkg -l "openssh-server" | grep "^i") && echo "installé" || echo "non installé"

ii openssh-server 1:8.0p1-6build1 amd64 secure shell (SSH) server, for secure access
from remote machines installé

```
</ol>


<h1>Exercice 4</h1>

<ol>
 
<li><h3>Lister les programmes livrés avec coreutils. En particulier, on remarque que l’un deux se nomme [. De quoi s’agit-il ?<h3></li>
 
```bash
dpkg -L coreutils
```

La commande `"[" `est la commande de test. Elle permet d'écrire des conditions dont ont peut voir les résultats à l'aide des opérateur `&&` et `||`

</ol>

<h1>Exercice 5 - aptitude</h1>

<ol>
 
<li><h3>Installez les paquets emacs et lynx à l’aide de la version graphique d’aptitude (et prenez deux minutes pour vous renseigner et tester ces paquets).<h3></li>
 
*Pour installer le paquet `emacs`, via `aptitude`, il faut d'abord installer le paquet aptitude en luimême puis une fois sur aptitude on fait une recherche en tapant :`/emacs` puis il faut appuyer sur n pour passer aux autres pages afin de trouver le paquet nommé emacs seul, puis appuyer sur +
pour accepter d'installer le paquet, puis appuyer deux fois sur g pour confirmer et lancer
l'installation. Ensuite si l'on tape emacs puis entrer nous avons bien accès au programme emacs sur notre machine*

</ol>

<h1>Exercice 6. Installation d’un paquet par PPA</h1>

<ol>
 
<li><h3>Certains logiciels ne figurent pas dans les dépôts officiels. C’est le cas par exemple de la version ”officielle” de Java depuis qu’elle est développée par Oracle. Dans ces cas, on peut parfois se tourner vers un ”dépôt personnel” ou PPA.<h3></li>

 *1. Installer la version Oracle de Java (avec l’ajout des PPA)*
 
```bash
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java21-installer
```

*2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?*

*Nous remarquons bien que un nouveau fichier a été créé*
 
```bash
[11:23]-[root]@ubuntu-/etc/apt/sources.list.d: ll
total 12
drwxr-xr-x 2 root root 4096 avril 1 11:16 ./
drwxr-xr-x 7 root root 4096 avril 1 11:16 ../
-rw-r--r-- 1 root root 136 avril 1 11:16 linuxuprising-ubuntu-java-eoan.list
```

</ol>

<h1>Exercice 7. Installation d’un logiciel à partir du code source</h1>

<ol>
 
<li><h3>Lorsqu’un logiciel n’est disponible ni dans les dépôts officiels, ni dans un PPA, ou encore parce qu’on souhaite n’installer qu’une partie de ses fonctionnalités, on peut se tourner vers la compilation du code source. C’est ce que nous allons faire ici, avec le programme cbonsai (https://gitlab.com/jallbrit/cbonsai)<h3></li>

 *1. Commencez par cloner le dépôt git suivant :*
 
```bash
git clone https://gitlab.com/jallbrit/cbonsai
```
*Ceci permet de récupérer en local le code source du logiciel cbonsai.*

*2. Rendez vous dans le dossier cbonsai. Un fichier README.md) est livré avec les sources, et vous explique
comment compiler le programme (vous pouvez installer un lecteur de Markdown pour Bash, comme
mdless pour vous faciliter la lecture de ce type de fichiers).*

*Un fichier Makefile est également présent. Un Makefile est un fichier utilisé par l’outil make, et
contient toutes les directives de compilation d’un logiciel. Un Makefile définit un certain nombre de
règles permettant de construire des cibles. Les cibles les plus communes étant install (pour la compilation
et l’installation du logiciel) et clean (pour sa suppression).*

*En suivant les consignes du fichier README.md, et en installant les éventuels paquets manquants, compilez
ce programme et installez le en local.*

*3. Malheureusement, cette installation “à la main” fait qu’on ne dispose pas des bénéfices de la gestion
de paquets apportés par dpkg ou apt. Heureusement, il est possible de transformer un logiciel installé
“à la main” en un paquet, et de le gérer ensuite avec apt ; c’est ce que permet par exemple l’outil
checkinstall.*

*4. Recommencez la compilation à l’aide de checkinstall :*
 
```bash
sudo checkinstall
```
*Un paquet a été créé (fichier xxx.deb), et le logiciel est à présent installé (tapez cbonsai depuis n’importe
quel dossier pour vous en assurer) ; on peut vérifier par exemple avec aptitude qu’il provient bien du paquet
qu’on a créé avec checkinstall.
Vous pouvez à présent profiter d’un instant de zenitude avant de passer au dernier exercice.*

</ol>

<h1>Exercice 8. Création de dépôt personnalisé</h1>

<ol>
 
<li><h3>Dans cet exercice, vous allez créer vos propres paquets et dépôts, ce qui vous permettra de gérer les programmes que vous écrivez comme s’ils provenaient de dépôts officiels.<h3></li>

<li><h3>Création d’un paquet Debian avec dpkg-deb<h3></li>

*1. Dans le dossier scripts créé lors du TP 2, créez un sous-dossier origine-commande où vous créerez un sous-dossier DEBIAN, ainsi que l’arborescence usr/local/bin où vous placerez le script écrit à l’exercice 2*

*2. Dans le dossier DEBIAN, créez un fichier control avec les champs suivants :*
 
```bash
Package: origine-commande #nom du paquet
Version: 0.1 #numéro de version
Maintainer: Foo Bar #votre nom
Architecture: all #les architectures cibles de notre paquet (i386, amd64...)
Description: Cherche l'origine d'une commande
Section: utils #notre programme est un utilitaire
Priority: optional #ce n'est pas un paquet indispendable
```
*3. Revenez dans le dossier parent de origine-commande (normalement, c’est votre $HOME) et tapez la
commande suivante pour construire le paquet :*

```bash
dpkg-deb --build origine-commande
```

*Félicitations ! Vous avez créé votre propre paquet !*

<li><h3>Création du dépôt personnel avec reprepro<h3></li>

*1. Dans votre dossier personnel, commencez par créer un dossier repo-cpe. Ce sera la racine de votre dépôt*
*. Ajoutez-y deux sous-dossiers : conf (qui contiendra la configuration du dépôt) et packages (qui contiendra nos paquets)*
*3. Dans conf, créez le fichier distributions suivant :*

```bash
Origin: Un nom, une URL, ou tout texte expliquant la provenance du dépôt
Label: Nom du dépôt
Codename: Nom de code de la distribution cible (impish, jammy, kinetic...)
Architectures: i386 amd64 #(architectures cibles)
Components: universe #(correspond à notre cas)
Description: Une description du dépôt
```

*4. Dans le dossier repo-cpe, générez l’arborescence du dépôt avec la commande*

```bash
reprepro -b . export
```
5. Copiez le paquet origine-commande.deb créé précédemment dans le dossier packages du dépôt, puis, à la racine du dépôt, exécutez la commande afin que votre paquet soit inscrit dans le dépôt.*

```bash
reprepro -b . includedeb nom_de_la_distribution origine-commande.deb
```

*6. Il faut à présent indiquer à apt qu’il existe un nouveau dépôt dans lequel il peut trouver des logiciels. Pour cela, créez (avec sudo) dans le dossier /etc/apt/sources.list.d le fichier repo-cpe.list contenant :*

```bash
deb file:/home/VOTRE_NOM/repo-cpe nom_de_la_distribution multiverse
```

*(cette ligne reprend la configuration du dépôt, elle est à adapter au besoin)*
*7. Lancez la commande sudo apt update.
Féliciations ! Votre dépôt est désormais pris en compte ! ... Enfin, pas tout à fait... Si vous regardez la sortie d’apt update, il est précidé que le dépôt ne peut être pris en compte car il n’est pas signé. La signature permet de vérifier qu’un paquet provient bien du bon dépôt.On doit donc signer notre dépôt.*

<li><h3>Signature du dépôt avec GPG<h3></li>
 
<li>GPG est la version GNU du protocole PGP (Pretty Good Privacy), qui permet d’échanger des données de manière sécurisée. Ce système repose sur la notion de clés de chiffrement asymétriques (une clé publique et une clé privée)</li>

*1. Commencez par créer une nouvelle paire de clés avec la commande*

`gpg --gen-key`

Attention ! N’oubliez pas votre passphrase !!!

*2. Ajoutez à la configuration du dépôt (fichier distributions la ligne suivante :*

`SignWith: yes`

*3. Ajoutez la clé à votre dépôt :*

`reprepro --ask-passphrase -b . export`

Attention ! Cette méthode n’est pas sécurisée et obsolète ; dans un contexte professionnel, on utiliserait plutot un `gpg-agent`.

*4. Ajoutez votre clé publique à votre dépôt avec la commande*

`gpg --export -a "auteur" > public.key`

*5. Enfin, ajoutez cette clé à la liste des clés fiables connues de apt :*

`sudo apt-key add public.key`

*Félicitations ! La configuration est (enfin) terminée ! Vérifiez que vous pouvez installer votre paquet comme n’importe quel autre paquet.*
 
</ol>

