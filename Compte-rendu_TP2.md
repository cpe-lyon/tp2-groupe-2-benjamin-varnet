# Compte rendu de TP2

**Toutes les commandes ont été entrées en root !**

## Exercice 1. Variables d’environnement

**Question 1 :** Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?<br/>
La variable PATH indique a BASH ou trouver les commandes entrées par l'utilisateur :
Pour trouver cette variable, nous avons entré les commandes suivantes :
 printenv | grep PATH  
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin

BASH regarde successivement dans chaque dossier pour trouver les commandes.

**Question 2 :** Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel ? <br/>
La variable d'environnement est la variable $HOME : 
printenv | grep HOME
HOME=/home/julien

**Question 3 :** Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _. <br/>

La variable LANG, donne la langue choisie par l'utilisateur et qui est utilisé par le système.
La variable PWD, fait la même chose que la commande pwd mais la sauvegarde dans une variable d'environnement 
La variable OLDPWD, donne le répertoire où l'on était précédemment 
La variable SHELL, donne le shell utilisé par le système (le shell BASH dans notre cas /bin/bash)
La variable _ est la où se situe la variable printenv. 

**Question 4 :** Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe. <br/>
Voici les commandes utilisées pour créer la variable, puis pour l'afficher :

$ local MY_VAR=toto
$ echo $MY_VAR
toto

**Question 5 :** Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale. <br/>

La commande bash permet d'utiliser le bash comme interpréteur de commande, mais n'a aucun effet dans notre cas car nous sommes déjà en shell bash.
La commande bash créée également (sans que nous puissions le voir) une nouvelle session utilisateur nous n'avons donc plus accès à nos variables d'environnement, et lorsque nous tapons exit, nous avons de nouveau accès à nos variables.

**Question 6 :** Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez. <br/>

`$ export MY_VAR=toto`
`$ printenv | grep MY_VAR`
`MY_VAR=toto`


Ces commandes ci-dessus, permettent de passer la variable en variable d'environnement et de ce fait, même si nous changeons de sessions, les variables sont toujours visibles.

**Question 7 :** Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte. <br/>

Nous avons donc ci-dessous, créé la variable d'environnement NOM:

julien@ubuntu:~$ export NOMS="Julien Clément Benjamin"
julien@ubuntu:~$ printenv | grep NOMS
NOMS=Julien Clément Benjamin

**Question 8 :** Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS. <br/>

La commande ci-dessous, créé et affiche la variable
julien@ubuntu:~$  echo "Bonjour à vous trois, $NOMS !"
Bonjour à vous trois, Julien Clément Benjamin !

**Question 9 :** Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande `unset` ? <br/>

Une variable passer en vide nous affiche la variable égal à rien.
L'utilisation de la commande unset, décomissionne l'ancienne valeur de notre variable.

**Question 10 :** Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash) <br/>

A l'aide de la commande `echo` nous obtenons les résultats suivants :
julien@ubuntu:~$ echo '$HOME ='"$HOME"
$HOME =/home/julien

### Programmation Bash
Vous enregistrerez vos scripts dans un dossier script que vous créerez dans votre répertoire personnel.
Tous les scripts sont bien entendu à tester.
Ajoutez le chemin vers script à votre PATH de manière permanente

Voilà le résultat de la partie programmation :

julien@ubuntu:~/scripts$ export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/julien/scripts
julien@ubuntu:~/scripts$ printenv | grep PATH
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/julien/scripts


## Exercice 2. Contrôle de mot de passe

Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.

Le script que nous avons réalisé est le suivant : 

>#!/bin/bash
>#######################
>#   Authors : Julien  #
>#######################
>
>read -s -p "Saissisez votre mots de passe :" password_readen
>password="benji"
>if [[ $password_readen == $password ]]; then
>    echo -e "\n Vous avez entré(e) le bon mot de passe"
>else
>    echo -e " \n Vous avez entré(e) le mauvais mot de passe"
>    exit 1
>fi
>exit



## Exercice 3. Expressions rationnelles

Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre
est un nombre réel :
function is_number()
{
re='^[+-]?[0-9]+([.][0-9]+)?$'
if ! [[ $1 =~ $re ]] ; then
return 1
else
return 0
fi
}
Il affichera un message d’erreur dans le cas contraire.
