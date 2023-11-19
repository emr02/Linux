# TP 2 - Bash

Ce deuxième TP a pour but d’approfondir nos connaissances sur Bash, les variables d’environnement et l’automatisation de tâches via la programmation de scripts.

## Exercice 1. Variables d’environnement
	
#### Question 1 : Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?
Il les trouve dans tous les dossiers contenus dans la variable $PATH à savoir : 
* /usr/local/sbin
* /usr/local/bin
* /usr/sbin
* /usr/bin
* /sbin
* /bin
  
#### Question 2 : Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel ?
Il s'agit de la variable d'environnement $HOME. On peut d'ailleurs l'afficher dans le terminal en saisissant la commande ```printenv HOME```
      
#### Question 3 : Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.
* "LANG" contient la langue sélectionnée pour le système, "fr_FR.UTF-8" dans notre cas.
* "PWD" contient le chemin du répertoire courant, "/bin" dans notre cas.
* "OLDPWD" contient le chemin de l'ancien répertoire courant "/fs03/share/users/pierre.neyroud/home" dans notre cas.
* "SHELL" contient l'interpréteur de commande utilisateur défini dans "/etc/passwd".
* "_" affiche l'emplacement de la commande printenv.
		
#### Question 4 : Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.
On va tout d'abord créer la variable locale MY_VAR : 
```bash
MY_VAR="abcd"
```   
On va ensuite l'afficher à l'aide du $ (on aurait MY_VAR d'affiché sinon) :
```bash
echo $MY_VAR
```
      
#### Question 5 : Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale.
Taper la commande ```bash``` va lancer un nouvel interpréteur de commande (un shell sh amélioré).  
La saisie de la commande ```echo $MY_VAR``` ne va donc rien donner puisque cette variable à été a été déclarée en tant que variable locale !  
La saisie de la commande ```echo $SHLVL``` est utilie puisqu'elle nous renvoie le niveau dans lequel nous nous trouvons actuellement.  
Enfin, en tapant ```exit``` nous revenons à la couche précédente, nous redonnant ainsi accès à la variable "MY_VAR".

	
#### Question 6 : Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.
On va saisir la suite de commande : 
```bash
export MY_VAR="abcd" 
printenv MY_VAR
```
La variable existe bien car elle est devenu une variable d'environnement.

#### Question 7 : Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.
On va saisir la suite de commande : 
```bash
export NOMS="ospina neyroud 
printenv NOMS
```
L'affectation est correcte "ospina neyroud".

#### Question 8 : Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS.
On va créer un script bash "nano script.sh" avec le contenu suivant :
```sh
#!/bin/bash
echo 'Bonjour à vous deux,' $NOMS '!'
```
On le rend ensuite executable avec la commande ```chmod u+x script.sh``` et on l'execute avec ```./script.sh```.
      
#### Question 9 : Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande unset ?
La commande "unset" supprime la variable d'environnement du système alors que donner une valeur vide à une variable ne la supprime pas mais laisse un contenu vide.

#### Question 10 : Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash).
On saisit la commande : 
```bash
echo '$HOME' = "$HOME"
```
On obtient :
```bash
$HOME =  /fs03/share/users/pierre.neyroud/home
```
Remarque : on utilise des "" pour que la variable soit interprétée comme telle et non comme un texte.
	
#### Vous enregistrerez vos scripts dans un dossier script que vous créerez dans votre répertoire personnel. Tous les scripts sont bien entendu à tester. Ajoutez le chemin vers script à votre PATH de manière permanente.
On créer un dossier "script" à l'aide de la commande ```mkdir```.  
Ensuite nous allons ajouter le chemin vers ce dossier dans la variable d'environnement PATH:
* Entrer dans le fichier ```vim .bashrc```  
* Ajouter à la fin du fichier ```echo PATH=$PATH:~/script >> ~/.bashrc```  
* Relire le bashrc à l'aide de la source ```~/.bashrc.```  


## Exercice 2. Contrôle de mot de passe
	
#### Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.

```sh   
#!/bin/bash

PASSWORD="admin"

read -p 'Saisissez le mot de passe ' -s mdp

if ["$PASSWORD" = "$mdp"]; then
	echo "Bon mot de passe :)"
else
	echo "Mauvais mot de passe :("
fi
```

## Exercice 3. Expressions rationnelles
	
#### Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre réel :

```sh   
#!/bin/bash

function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		return 1
	else
		return 0
	fi
}

read -p "Saisissez un nombre " nb
is_number $nb
if [ $? -eq 0 ]; then
	echo "C'est bien un nombre :)"
else
	echo "Ce n'est pas un nombre :("
fi
```
		
## Exercice 4. Contrôle d’utilisateur
	
#### Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”, où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer automatiquement)

```sh   
#!/bin/bash

if [ -z $1 ];then
	echo "Utilisation : $0 nom_utilisateur"
else
	for user in $(cut -d: -f1 /etc/passwd)
	do
		if [ $user = $1 ];then
			echo "Utilisateur trouvé :)"
			exit
		fi
	done
	echo "Utilisateur non trouvé :("
fi
```   
	
## Exercice 5. Factorielle

#### Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que l’utilisateur saisit toujours un entier naturel).

```sh   
#!/bin/bash

function fact()
{
  FACT=$FACT*$1
  $1="$1"-1
  if ! [ $1 = 1 ]; then
    fact $1
  fi
}

export FACT=1
fact $1
echo "$FACT"
```
	
## Exercice 6. Le juste prix 
	
#### Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner. Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !” selon les cas (vous utiliserez $RANDOM).

```sh  
#!/bin/bash

prix=$(((RANDOM%1000)+1))

devine=-1

while [ $devine -ne $prix ]
do
read devine
if [ $devine -gt $prix ]; then
  echo 'C'est moins !'
fi
if [ $devine -lt $prix ]; then
  echo 'C'est plus !'
fi
done

echo 'Gagné !'
```

## Exercice 7. Statistiques

#### Question 1 : Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres sont bien des entiers.
#### Question 2 : Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT)
#### Question 3 : Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et stockées au fur et à mesure dans un tableau.

```sh
#!/bin/bash

function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		return 1
	else
		return 0
	fi
}

sum=0
count=0
min=$1
max=$1

while (("$#")); do
    is_number $1
    if [[ $? = 1 ]]; then
        echo Un des paramètres n\'est pas un nombre
        exit
    fi

    sum=$((sum + $1))
    count=$((count + 1))

    if [[ $1 -gt $max ]]; then
        max=$1
    elif [[ $1 -lt $min ]]; then
        min=$1
    fi
    shift
done

echo Min: $min
echo Max: $max
printf 'Moyenne: %.2f\n' $(echo "$sum / $count" | bc -l)
```

```sh
#!/bin/bash

function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		return 1
	else
		return 0
	fi
}

values=()
input="0"
index=0

while [ -n "$input" ]; do
    read -p "Entrez un nombre: " input

    if [ -n "$input" ]; then
        is_number $input
        if [[ $? = 1 ]]; then
            echo Ce n\'est pas un nombre !
        else
            values[$index]=$input
            index=$(( $index + 1 ))
        fi
    fi
done

sum=0
min=${values[0]}
max=${values[0]}

for n in ${values[@]}; do
    sum=$((sum + $n))
    count=$((count + 1))

    if [[ $n -gt $max ]]; then
        max=$n
    elif [[ $n -lt $min ]]; then
        min=$n
    fi
done

echo Min: $min
echo Max: $max
printf 'Moyenne: %.2f\n' $(echo "$sum / $index" | bc -l)
```
	
## Exercice 8. Pour les plus rapides

#### Écrivez un script qui affiche les combinaisons possibles de couleurs (cf. TP 1)
```sh
for i in $(seq 0 37); do
	for j in $(seq 40 47); do
		echo -e \\"e[0;"$j"mBash\\e[0m"
	done;
done;
```
	
	
