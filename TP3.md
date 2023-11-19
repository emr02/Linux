 # TP 3 - Utilisateurs, groupes et permissions

<h1>Exercice 1. Gestion des utilisateurs et des groupes</h1>

<ol>
<li><h3>Commencez par créer deux groupes groupe1 et groupe2<h3></li>

*création d'utilisateur*

 >`sudo adduser antoine`

*ajout groupe*

 >`sudo addgroup dev`
 >`sudo groupadd infra`

<li><h3>Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de
leur dossier personnel et avec bash pour shell
</h3></li>

*création d'un utilisateur u1 avec son dossier personnel et Sélectionne le shell à utiliser pour exécuter les commandes du terminal*

 >`-m => création du dossier personnel`

  >`--shell => indique l 'éxécution avec le bash`

 >`sudo useradd u1 -m --shell /bin/bash`

<li><h3>Placez les utilisateurs dans les groupes :
- u1, u2, u4 dans groupe1
- u2, u3, u4 dans groupe2
</h3></li>

*- Ajouter un utilisateur existant à un groupe (secondaire) existant*

>`usermod -a -G groupe1 u1`

<li><h3>Donnez deux moyens d’afficher les membres de groupe2</h3></li>

*affiche grace au chemin et a la commande grep qui récupère la valeur "groupe"*

>`cat etc/group | grep "groupe2"`

*installation du packet members pour afficher les nembres du groupes*

>`members "groupe2"`

<li><h3>Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire
de /home/u3 et /home/u4</h3></li>

*chown est la commande qui modifier  le propriétaire du fichier ici groupe1 deviens prop du fichier u2  *

>`chown  :groupe1 /home/u2`

<li><h3>Remplacez le groupe primaire des utilisateurs :
- groupe1 pour u1 et u2
- groupe2 pour u3 et u4</h3></li>

*usermod permet modifier le groupe primaire d’un utilisateur*

>`sudo usermod -g groupe1 u1,u2`

<li><h3>Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et
mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier
associé</h3></li>

*création d'un fichier groupe1*

>`sudo mkdir group1`

*chmod permet de modifier les droits existant sur
les fichiers 1 grace au valeur numérique 770 on donneras les droits sur se dossiers*

>`chmod 770 groupe1`

<li><h3>Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer
ou supprimer ce fichier ?
</h3></li>

*grace a sticky bit  que seul les propriétaire de se fichiers peuvent le modifier ect ici le propriétaire de se fichier est groupe1 donc tout les nembres du groupe 1 peuvent le modifier*

>`chmod +t groupe1`


<li><h3>Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?</h3></li>

*avec su on prend l’identité de quelqu’un d’autre, pas avec sudo*

>`su u1`

*non car nous n'avons pas configurer de mot de passe*

<li><h3>Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son
compte</h3></li>

*Nous allons configurer un mpd au compte*

>`sudo passwd u1`

*on vérifie en se connectant*

>`su u1`

<li><h3>Quels sont l’uid et le gid de u1 ?
</h3></li>

*grace id nomUtilisateurs nous allons récupérer sont id*

>`id u2`

<li><h3>Quel utilisateur a pour uid 1003 ?</h3></li>

*on utilise la commande getent pour rechercher dans un groupe la personne qui a pour uid=1003*

>`getent group groupe1 1004`

<li><h3>Quel est l’id du groupe groupe1 ?</h3></li>

*soit on utilise getent pour récupérer l'id du groupe soit on se déplace dans le dossier groupe pour regarder sont id*

>`getent group groupe1`

<li><h3>Quel groupe a pour guid 1002 ?</h3></li>

*grace a getent il suffit de renseigner l id du group pour qu'il nous le trouve*

>`getent group 1003`

<li><h3>Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.</h3></li>

*la commande gpasswd est utilisée pour  administrer les groups donc remove ect -d = delete*

>`sudo gpasswd -d u3 groupe1 `

*on ne peut pas enlever sont utilisateur car c'est sont unique groupe*

<li><h3>Modifiez le compte de u4 de sorte que :
— il expire au 1
er juin 2020
— il faut changer de mot de passe avant 90 jours
— il faut attendre 5 jours pour modifier un mot de passe
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe
— le compte sera bloqué 30 jours après expiration du mot de pass</h3></li>

*--expiredate = set date*

*chage -m = set j update mdp*

*chage -w = set j expire mdp*

*chage -I = set j compte bloqué*

<code>
sudo usermod --expiredate 2020-06-01 u4
sudo chage -m 90 u4
sudo chage -W 14 u4
sudo chage -I 30 u4
</code>

<li><h3>Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?</h3></li>

<code>
grep root /etc/passwd
root:x:0:0:root:/root:/bin/bash
</code>

*Ou bien*

<code>
sudo echo $SHELL
C'est /bin/bash
</code>

<li><h3>à quoi correspond l’utilisateur nobody ?</h3></li>

<code>
grep nobody /etc/passwd
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
</code>

*Nobody est un utilisateur qui n'a aucun droit sous linux, les droits minimun. Il a donc que les droits défini dans other des fichiers.*

<li><h3>Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ?
Quelle commande permet de forcer sudo à oublier votre mot de passe ?
 minutes par défaut.</h3></li>
 
*Selon le manuel de la commande sudo, celle-ci conserve le mot de passe en mémoire pendant 15
for 15 minutes. Cela peux être réinitialiser avec la commande:*

>` serveur@serveur:~$ sudo -K `

</ol>

<h1>Exercice 2. Gestion des permissions</h1>

<ol>
<li><h3>Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques
lignes de texte. Quels sont les droits sur test et fichier ?
</h3></li>

*création d'un fichier nommé fichier et d'un dossier test et *

<code>
sudo mkdir test
sudo touch fichier
</code>

*affichage des droits du dossier*

<code>
ls -l test
ls -l test/fichier
</code>

*-rw--r--r 1*
<li><h3>Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?</li></h3>

*retire tout les droit du fichier grace a chmod (rwx droit écriture ect)(ugo user group ect)*
<code>
chmod 000 test/fichier
ou
chmod ugo-rwx test/fichier
</code>

*test du droit de regard avec mon utilisateur donc je n'est plus les droit*
<code>
cat test/fichier
</code>

*test en mode root*

<code>
sudo su
cat test/fichier
</code>

*donc la on arrive a le lire*

<li><h3>Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?</h3></li>

*on est redonnez les droit d'écriture sur notre fichier*

<code>
sudo chmod 300 test/fichier
</code>

*maintenant on va écrire dans notre fichier*

<code>
echo "droit ecriture" > test/fichier
</code>

*maintenant on va afficher notre texte mais comme nous n'avons pas les droit de regard nous allons le faire en mode root*

<code>
sudo su
cat test/fichier
</code>

<li><h3>Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez</li></h3> 

*nous n'avons pas les droits de lecture sur le fichier donc on ne peut pas l'éxécuter même avec sudo*

<code>
./test/fichier
</code>

<li><h3>Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le
contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ?
Rétablissez le droit en lecture sur test</h3></li>

*on enlève les permissions du répertoir test *
<code>
sudo chmod u-r ../test
</code>
*puis on execute le fichier*
<code>
./fichier
</code>
*on ne peut pas l'éxecuter car on ne peut pas lire les fichiers dans ce répertoire donc on ne peut pas les éxécuter (on ne peut même pas faire de ls car on na pas le droit de lister les fichiers)*

<li><h3>Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?</h3></li>

*création du fichier nouveau et un répertoire*

<code>
cd /test
touch nouveau
mkdir sstest
</code>

*on retire les droits au fichier nouveau et au repertoire*

<code>
chmod u-w nouveau
chmod u-w  ../test
</code>

*on ne peut pas écrire dans notre fichier avec nano fichier  même si on re donne les droits d'écriture au répertoire, le répertoire nes't pas prioritaire*

<code>
chmod u+w ../test
nano nouveau 
permission denied
</code>

<li><h3>Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?</h3></li>

*on se déplace dans notre répétoire personnelle et on lui retire tout les droits*

<code>
cd test
chmod u-x ../test
</code>

*nous allons tester les permissions*

<code>
cd test
ls test
</code>

*nous n'avons aucun droit sur le répertoir donc aucune commande marche*

<li><h3>Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui
à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire
test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des
droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd
..” ? Pouvez-vous donner une explication ?</h3></li>

*on allons rétablir les droit d'éxécution de notre répertoire*

<code>
cd test/
chmod u-x ../test
</code>

*nous allons tester les permissions*

<code>
cd sstest
ls
</code>

*on remarque que notre répertoire courant est trés important  car les dossiers fils hérite de leur droits*


<li><h3>Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants
pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.</h3></li>

*on redonne les droits au fichier*

<code>
chmod 650 fichier
ll
</code>

*on remarque que le total des droits de se fichier est égal a 16*

<li><h3>Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture,
ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
</h3></li>

*on va créer un repartoir et un nouveau fichier*

<code>
touch fichier
mkdir folder
</code>

*création d'un umask 177 car sticky bit pour les autorisation d'écriture et de lecture*

<code>
umask 1 077
pour voire les droit
ll
</code>

<li><h3>Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos répertoires, mais n’autorise que vous à écrire. Testez sur un nouveau fichier et un nouveau répertoire.</h3><li>

*definition d'un nouveau umask*
<code>
umask 011
affiche les droits des ficher et dossier courant :
ll
</code>


<li><h3>Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.</h3></li>

*definition d'un nouveau umask*

<code>
umask 037
affiche les droits des ficher et dossier courant:
ll
</code>

<li><h3>Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) :</h3></li>

<code>
  - chmod u=rx,g=wx,o=r fic `534`
  - chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x--- `501`
  - chmod 653 fic en sachant que les droits initiaux de fic sont 711 `Hein ?`
  - chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x--- `520`
 </code>

<li><h3>Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?</h3></li>

<code>
 serveur@serveur:~/folder$ ll /etc/passwd
-rw-r--r-- 1 root root 1801 sept. 30 15:08 /etc/passwd
 </code>
 
</ol>

