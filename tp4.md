
## Exercice 1. Gestion des utilisateurs et des groupes 
#### Commencez par créer deux groupes groupe1 et groupe2
Avec la commande ``addgroup groupe1 && addgroup groupe2``

#### Créez ensuite 4 utilisateurs u1, u2, u3, u4 avec la commande useradd, en demandant la création de leur dossier personnel et avec bash pour shell
```
 sudo adduser u1 --home /home/u1 --shell $SHELL
 sudo adduser u2 --home /home/u2 --shell $SHELL
 sudo adduser u3 --home /home/u3 --shell $SHELL
 sudo adduser u4 --home /home/u4 --shell $SHELL
```

#### Placez les utilisateurs dans les groupes : - u1, u2, u4 dans groupe1 - u2, u3, u4 dans groupe2

  - u1, u2, u4 dans groupe1
  
```  
serveur@serveur:~$ sudo usermod -a -G groupe1 u1
serveur@serveur:~$ sudo usermod -a -G groupe1 u2
serveur@serveur:~$ sudo usermod -a -G groupe1 u4
```

  - u2, u3, u4 dans groupe2

```
serveur@serveur:~$ sudo usermod -a -G groupe2 u2
serveur@serveur:~$ sudo usermod -a -G groupe2 u3
serveur@serveur:~$ sudo usermod -a -G groupe2 u4
```

#### Donnez deux moyens d’afficher les membres de groupe2
Avec la commande : ``grep '^groupe2' /etc/group``

####  Faites de groupe1 le groupe propriétaire de /home/u1 et /home/u2 et de groupe2 le groupe propriétaire de /home/u3 et /home/u4
Avec la commande ``chown :[nom_groupe] [chemin_dossier]``

####  Remplacez le groupe primaire des utilisateurs :
  - groupe1 pour u1 et u2 
Avec la commande : ``usermod u1 -g groupe1``  puis  
``usermod u2 -g groupe1``

- groupe2 pour u3 et u4 
Avec la commande : ``usermod u3 -g groupe2`` puis ``usermod u4 -g groupe2``

#### Créez deux répertoires /home/groupe1 et /home/groupe2 pour le contenu commun aux groupes, et mettez en place les permissions permettant aux membres de chaque groupe d’écrire dans le dossier associé.
Avec la commande ``chmod 720 [nom_dossier]``

#### • Comment faire pour que, dans ces dossiers, seul le propriétaire d’un fichier ait le droit de renommer ou supprimer ce fichier ?
Grace à la mise en place d'un sticky bit.
Avec par exemple la commande ```chmod 1720 [nom_dossier]``

#### Pouvez-vous vous connecter en tant que u1 ? Pourquoi ?
Non on ne peut pas se connecter sur u1 car le mot de passe de login n'existe pas car la commande ``useradd`` ne génère pas automatiquement de mot de passe à la création d'un utilisateur.

#### Activez le compte de l’utilisateur u1 et vérifiez que vous pouvez désormais vous connecter avec son compte.
Pour activer le compte u1, il faut utiliser la commande ``passwd u1``
Il faut initialiser un nouveau mot de passe. 
Il est maintenant possible de se connecter.

#### Quels sont l’uid et le gid de u1 ?
Grace à la commande ``id u1`` on peut voir que son uid et gid est 1001.

#### Quel utilisateur a pour uid 1003 ?
L'utilisateur u3 à pour uid 1003 car il est le troisième utilisateur à avoir été créé.

#### Quel est l’id du groupe groupe1 ?
Le gid du groupe 1 est 1001 car il est le premier groupe à avoir été créé.

#### Quel groupe a pour gid 1002 ? (Rien n’empêche d’avoir un groupe dont le nom serait 1002...)
Avec la commande ``grep "1002" /etc/group`` on peut voir que c'est le groupe 2 qui possède le gid 1002

#### Retirez l’utilisateur u3 du groupe groupe2. Que se passe-t-il ? Expliquez.
Avec la commande ``gpasswd -d u3 groupe2`` on retire u3 du groupe 2. Il ne peut donc plus accéder au dossier groupe2 car celui-ci n'est plus dans le groupe2.

#### Modifiez le compte de u4 de sorte que : 
— il expire au 1 er juin 2019 
Avec la commande ``usermod --expiredate 06/01/2019 u4``
— il faut changer de mot de passe avant 90 jours 
``chage -M 90 u4``
— il faut attendre 5 jours pour modifier un mot de passe 
``chage -m 5 u4``
— l’utilisateur est averti 14 jours avant l’expiration de son mot de passe 
``chage -W 14 u4``
— le compte sera bloqué 30 jours après expiration du mot de passe
``chage -I 30 u4``

#### Quel est l’interpréteur de commandes (Shell) de l’utilisateur root ?
Avec la commande ``echo $SHELL`` ce qui donne /bin/bash

#### à quoi correspond l’utilisateur nobody ?
nobody est le nom conventionnel d'un compte d'utilisateur à qui aucun fichier n'appartient, qui n'est dans aucun groupe qui a des privilèges et dont les seules possibilités sont celles que tous les "autres utilisateurs" ont.

#### 19- Par défaut, combien de temps la commande sudo conserve-t-elle votre mot de passe en mémoire ? Quelle commande permet de forcer sudo à oublier votre mot de passe ?
Le temps par défaut est 15 minute.
La commande pour forcer sudo à oublier son mot de passe est `sudo -k`

## Exercice 2. Gestion des permissions
#### Dans votre $HOME, créez un dossier test, et dans ce dossier un fichier fichier contenant quelques lignes de texte. Quels sont les droits sur test et fichier ?
``mkdir Test && touch Fichier /home/Test``

#### Retirez tous les droits sur ce fichier (même pour vous), puis essayez de le modifier et de l’afficher en tant que root. Conclusion ?
``chmod 000 Fichier`` Il est possible de l'éditer car je suis en root.

#### Redonnez vous les droits en écriture et exécution sur fichier puis exécutez la commande echo "echo Hello" > fichier. On a vu lors des TP précédents que cette commande remplace le contenu d’un fichier s’il existe déjà. Que peut-on dire au sujet des droits ?
``chmod 300 Fichier``
``echo "echo Hello" > Fichier``

#### Essayez d’exécuter le fichier. Est-ce que cela fonctionne ? Et avec sudo ? Expliquez
Ça ne va pas marcher car on a pas les droits d’exécution. Cependant en Root ça va marcher

#### Placez-vous dans le répertoire test, et retirez-vous le droit en lecture pour ce répertoire. Listez le contenu du répertoire, puis exécutez ou affichez le contenu du fichier fichier. Qu’en déduisez-vous ? Rétablissez le droit en lecture sur test
```sudo chmod u-r ../test
ls 
cannot open directory '.': Permission denied
./fichier 
bash: ./fichier: Permission denied
``` 
#### Créez dans test un fichier nouveau ainsi qu’un répertoire sstest. Retirez au fichier nouveau et au répertoire test le droit en écriture. Tentez de modifier le fichier nouveau. Rétablissez ensuite le droit en écriture au répertoire test. Tentez de modifier le fichier nouveau, puis de le supprimer. Que pouvezvous déduire de toutes ces manipulations ?
```
nano nouveau
mkdir sstest
chmod u-w nouveau
chmod u-w ../test
nano nouveau
[ Error writing nouveau: Permission denied ]
chmod u+w ../test
nano nouveau
[ Error writing nouveau: Permission denied ] 
rm nouveau
rm: remove write-protected regular file 'nouveau'? yes
ls fichier  sstest
```
Il n'y a pas d'héritage de droits des dossiers pour les fichiers contenu à l'intérieur.

#### Positionnez vous dans votre répertoire personnel, puis retirez le droit en exécution du répertoire test. Tentez de créer, supprimer, ou modifier un fichier dans le répertoire test, de vous y déplacer, d’en lister le contenu, etc…Qu’en déduisez vous quant au sens du droit en exécution pour les répertoires ?
On ne peut rien faire, le droit d'exécution sur un Dossier est primordial si l'on souhaite travailler à l'intérieur.

#### Rétablissez le droit en exécution du répertoire test. Positionnez vous dans ce répertoire et retirez lui à nouveau le droit d’exécution. Essayez de créer, supprimer et modifier un fichier dans le répertoire test, de vous déplacer dans ssrep, de lister son contenu. Qu’en concluez-vous quant à l’influence des droits que l’on possède sur le répertoire courant ? Peut-on retourner dans le répertoire parent avec ”cd ..” ? Pouvez-vous donner une explication ?
La modification des droits s'applique instantanément, même si l'on est à l'intérieur du dossier en question, si le droit d'exécution est retiré, il est plus possible de faire quoi que ce soit.

#### Rétablissez le droit en exécution du répertoire test. Attribuez au fichier fichier les droits suffisants pour qu’une autre personne de votre groupe puisse y accéder en lecture, mais pas en écriture.
La commande ``chmod 740``

#### Définissez un umask très restrictif qui interdit à quiconque à part vous l’accès en lecture ou en écriture, ainsi que la traversée de vos répertoires. Testez sur un nouveau fichier et un nouveau répertoire.
Umask : 066

#### Définissez un umask très permissif qui autorise tout le monde à lire vos fichiers et traverser vos rép
Umask 022

#### Définissez un umask équilibré qui vous autorise un accès complet et autorise un accès en lecture aux membres de votre groupe. Testez sur un nouveau fichier et un nouveau répertoire.
Umask : 037

#### Transcrivez les commandes suivantes de la notation classique à la notation octale ou vice-versa (vous pourrez vous aider de la commande stat pour valider vos réponses) : 
- chmod u=rx,g=wx,o=r fic  = ``534``
- chmod uo+w,g-rx fic en sachant que les droits initiaux de fic sont r--r-x--- = ``602``
- chmod 653 fic en sachant que les droits initiaux de fic sont 711 = ``rw-r-x-wx``
- chmod u+x,g=w,o-r fic en sachant que les droits initiaux de fic sont r--r-x--- = ``570``

#### Affichez les droits sur le programme passwd. Que remarquez-vous ? En affichant les droits du fichier /etc/passwd, pouvez-vous justifier les permissions sur le programme passwd ?

``root@serveur:/home/serveur# ls -l /etc/passwd
-rw-r--r-- 1 root root 1781 sept. 30 15:04 /etc/passwd``
