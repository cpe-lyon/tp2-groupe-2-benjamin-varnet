# Compte rendu de TP2

**Toutes les commandes ont été entrées en root !**

## Exercice 1. Variables d’environnement

**Question 1 :** Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?<br/>
La variable PATH indique a BASH ou trouver les commandes entrées par l'utilisateur : <br/>
Pour trouver cette variable, nous avons entré les commandes suivantes : <br/>
 printenv | grep PATH  <br/>
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin <br/>

BASH regarde successivement dans chaque dossier pour trouver les commandes. <br/>

**Question 2 :** Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel ? <br/>
La variable d'environnement est la variable $HOME : <br/>
printenv | grep HOME <br/>
HOME=/home/julien <br/>

**Question 3 :** Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _ <br/>

La variable LANG, donne la langue choisie par l'utilisateur et qui est utilisé par le système. <br/>
La variable PWD, fait la même chose que la commande pwd mais la sauvegarde dans une variable d'environnement <br/>
La variable OLDPWD, donne le répertoire où l'on était précédemment <br/>
La variable SHELL, donne le shell utilisé par le système (le shell BASH dans notre cas /bin/bash) <br/>
La variable _ est la où se situe la variable printenv. <br/>

**Question 4 :** Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe. <br/>
Voici les commandes utilisées pour créer la variable, puis pour l'afficher : <br/>

$ local MY_VAR=toto <br/>
$ echo $MY_VAR<br/>
toto <br/>

**Question 5 :** Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale. <br/>

La commande bash permet d'utiliser le bash comme interpréteur de commande, mais n'a aucun effet dans notre cas car nous sommes déjà en shell bash. <br/>
La commande bash créée également (sans que nous puissions le voir) une nouvelle session utilisateur nous n'avons donc plus accès à nos variables d'environnement, et lorsque nous tapons exit, nous avons de nouveau accès à nos variables.<br/>

**Question 6 :** Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez. <br/>

`$ export MY_VAR=toto`<br/>
`$ printenv | grep MY_VAR`<br/>
`MY_VAR=toto`<br/>


Ces commandes ci-dessus, permettent de passer la variable en variable d'environnement et de ce fait, même si nous changeons de sessions, les variables sont toujours visibles.

**Question 7 :** Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte. <br/>

Nous avons donc ci-dessous, créé la variable d'environnement NOM: <br/>

$ export NOMS="Julien Clément Benjamin" <br/>
$ printenv | grep NOMS <br/>
NOMS=Julien Clément Benjamin <br/>

**Question 8 :** Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS. <br/>

La commande ci-dessous, créé et affiche la variable<br/>
julien@ubuntu:~$  echo "Bonjour à vous trois, $NOMS !"<br/>
Bonjour à vous trois, Julien Clément Benjamin !<br/>

**Question 9 :** Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande `unset` ? <br/>

Une variable passer en vide nous affiche la variable égal à rien.<br/>
L'utilisation de la commande unset, décomissionne l'ancienne valeur de notre variable.<br/>

**Question 10 :** Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash) <br/>

A l'aide de la commande `echo` nous obtenons les résultats suivants :<br/>
julien@ubuntu:~$ echo '$HOME ='"$HOME"<br/>
$HOME =/home/julien<br/>

### Programmation Bash 
Vous enregistrerez vos scripts dans un dossier script que vous créerez dans votre répertoire personnel.<br/>
Tous les scripts sont bien entendu à tester.<br/>
Ajoutez le chemin vers script à votre PATH de manière permanente <br/>

Voilà le résultat de la partie programmation :<br/>

$ export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/julien/scripts <br/>
$ printenv | grep PATH <br/>
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin:/home/julien/scripts<br/>


## Exercice 2. Contrôle de mot de passe

Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.<br/>

Le script que nous avons réalisé est le suivant : <br/>

>#!/bin/bash <br/>
>####################### <br/>
>##Authors : Julien  ## <br/>
>####################### <br/>
>
>read -s -p "Saissisez votre mots de passe :" password_readen <br/>
>password="benji" <br/>
>if [[ $password_readen == $password ]]; then <br/>
>    echo -e "\n Vous avez entré(e) le bon mot de passe" <br/>
>else <br/>
>    echo -e " \n Vous avez entré(e) le mauvais mot de passe" <br/>
>    exit 1 <br/>
>fi <br/>
>exit <br/>



## Exercice 3. Expressions rationnelles

Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre réel :<br/>
function is_number()
{
re='^[+-]?[0-9]+([.][0-9]+)?$'
if ! [[ $1 =~ $re ]] ; then
return 1
else
return 0
fi
}
Il affichera un message d’erreur dans le cas contraire.<br/>


## Exercice 4. Contrôle d’utilisateur

Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”, où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer automatiquement)<br/>

Le script est le suivant : <br/>
>!/bin/bash<br/>
>##############################################################################
> Script name   : test-user.sh                                               #<br/>
> Version	: 1.0                                                            #<br/>
> Date          : Fev.2020                                                   #<br/>
> Synopsys      : TP2                                                        #<br/>
> Who           : Julien                                                     #<br/>
>##############################################################################

>_NOM_SCRIPT="$0"<br/>
>_USER_SEARCH="$1"<br/>

>res=$(cut -d: -f1 /etc/passwd | grep "$_USER_SEARCH")<br/>
>if [[ $res == $_USER_SEARCH ]] ; then<br/>
>	echo -e "Le compte $_USER_SEARCH existe dans le système"<br/>
>	exit 0 <br/>
>else <br/>
>	echo -e "Le compte $_USER_SEARCH n'existe pas dans le système"<br/>
>	exit 1 <br/>
>fi <br/>


Le résultat du script est le suivant : <br/>

$ test-user.sh julien <br/>
Le compte julien existe dans le système <br/>
$ test-user.sh benji<br/>
Le compte benji n'existe pas dans le système <br/>

