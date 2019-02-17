QUERON Amaury et SINNIG Solène

**Administration Système sous Linux (Ubuntu Server)**


#Prise en main de l’interpréteur de commandes


##Manuel 


Afin de connaître le rôle d'une commande, on utilise la commande *man* suivi du nom de la commande. Ici : *man which*

La commande which sert à envoyer le chemin des fichiers exécutés dans l'environnement.

On souhaite chercher le mot option dans le manuel, on utilise donc la touche */*.

Pour quitter le manuel, on appuie sur la touche *q*.

On souhaite afficher la première page de la section 6 : *man 6 intro*
Cette page affiche le man de la section 6 qui concerne les jeux.


##Navigation dans l’arborescence des fichiers


Pour accéder au dossier /var/log on tape la commande *cd /var/log* et pour remonter dans le dossier parent (/var) *cd ..* .
Pour retourner dans le dossier personnel on tape *cd ~*. Si l'on veut retourner dans le dossier où nous étions précédemment, il suffit de taper *cd -*.
Si on tape la commande *cd /root* pour accéder au dossier root, ça ne fonctionne pas, il faut utiliser le mode superutilisateur mais si l'on tape *sudo cd /root* cela ne fonctionne pas non plus. *cd* est une commande interne du shell tandis que *sudo* non. 

On souhaite ensuite réaliser l'arborescence demandée :
on crée deux dossiers dans notre dossier personnel avec la commande *mkdir*. Ensuite, on va dans ces dossiers pour créer des fichiers : *cd dossier1* puis *touch fichier1*. On vérifie avec *ls* si notre fichier existe bien. On se rend ensuite dans dossier2 *cd dossier2* pour créer deux dossiers (2.1 et 2.2) puis deux nouveaux fichiers dans le dossier2.2 (fichiers 2 et 3).

Pour supprimer un dossier, la commande *rm* ne fonctionne pas car elle ne sert que pour les fichiers, on doit utiliser la commande *rmdir* mais le dossier doit être vide. Cela ne fonctionne pas pour le dossier2 car il contient des documents, pour le supprimer en une seule commande i lfaut taper *rm -r dossier2*.


##Commandes importantes


Pour afficher l'heure *date +%T* (juste *date* si on veut la date et l'heure). La commande time affiche combien de temps elle a mis pour exécuter la commande qui suit (ex *time ls*) en affichant le temps CPU. 

La commande *la* sert à afficher tous les fichiers y compris les fichiers cachés qui commencent par un point.

Pour savoir où se situe le programme ls on tape *which ls*, i lse situe dans le dossier bin. 

La commande *ll* est un raccourci de la commande *ls -alF* qui sert à afficher les informations de manière détaillée en long format avec les droits d'accès aux fichiers.

Pour afficher les dossiers contenus dans le dossier /bin on tape *ls /bin*.
La commande *ls ..* affiche le contenu du dossier parent.

Pour afficher le chemin complet du dossier courant on tap *pwd*.

La commande *echo 'yo' > plop* permet de créer un fichier plop contenant le mot yo. La commande *echo 'yo' >> plop* permet également d'écrire yo dans le fichier plop mais sans écraser le premier yo (si l'on tape la commande 2 fois de suite) ce qui n'est pas le cas de la première commande.

La commande *file* permet de donner le type du fichier. 

Nous créons un fichier toto *echo 'Hello Toto !' > toto* puis on crée un lien *ln toto titi* vers titi, cela crée un fichier titi qui contient le même contenu que toto. Si l'on modifie le contenu de toto, titi est modifié aussi. Si l'on supprime toto, titi n'est pas supprimé et garde son contenu.

On crée maintenant un lien symbolique avec *ln -s titi tutu* Si l'on modifie titi, tutu est modifié aussi et inversement. Si l'on supprime l'un l'autre est supprimé.

On souhaite afficher à l'écran le fichier /var/log/syslog mais son contenu est tellement important qu'il défile automatiquement, pour interrompre le défilement il faut faire *Ctrl+S* pour le mettre en pause puis *Ctrl+Q* pour le reprendre.
Pour afficher les 5 premières lignes du fichier on tape *head -n 5 /var/log/syslog* puis *tail -n 15 /var/log/syslog* pour les 15 dernières. Pour lire entre lignes 10 et 20 i lfaut taper *head -n 10 /var/log/syslog | tail -n 20*.

La commande *dmesg* affiche les opérations du noyau (kernel) et grâce à *less* le fichier peut être lu ligne par ligne au moment de l'affichage.

Le fichier /etc/passwd contient les informations qui concernent les groupes et les utilisateurs. En tapant *man 5 passwd* on arrive sur une page du manuel qui apporte des précisions sur ce fichier.
On souhaite affiche seulement la première colonne triée par ordre alphabétique inverse : *cut -c1 /etc/passwd | sort -r* ; *cut -c1* pour récupèrer seulement la première colonne et *sort -r* pour trier par ordre alphabétique inverse.

On souhaite connaître le nombre d'utilisateurs : la commande *wc -l* permet de compter le nombre de lignes du fichier. Pour obtenir le nombre d'ulisateurs il faut donc taper *wc -l /etc/passwd*

La commande *man -k 'conversion' | wc -l* affiche le nombre de pages du manuel qui comportent le mot conversion : l'option *-k* sert à rechercher un mot clé dans les pages de description.

On souhaite rechercher tous les fichiers se nommant *passwd* présents sur la machine à l'aide de la commande *find* : *find -name passwd*

On modifie la commande précédente pour que la liste des fichiers trouvés soit enregistrée dans le fichier **~/list_passwd_files.txt** et que les erreurs soient redirigées vers le fichier spécial **/dev/null**. On utilise l'opérateur > pour rediriger le flux de la commande et l'opérateur 2> pour rediriger uniquement les erreurs : *find / -name passwd > list_passwd_files.txt 2> /dev/null*.

Pour trouver le fichier history.log on utilise la commande *locate history.log /var/log/apt/history.log*.

*grep* permet de chercher une chaine dans un fichier et renvoie les lignes où apparaissent les occurences.
*grep ll .bashrc* permet de trouver là où se situe la définition de l'alias ll dans le fichier .bashrc se situant dans notre dossier d'accueil.


#Personnalisation du shell


Nous souhaitons en environnement plus agréable, pour cela nous ajoutons des couleurs au shell. Nous modifions donc le fichier ~/.bashrc.
Comme demandé nous modifions la mise en forme de l'invite de commande : *PS1='${debian_chroot:+($debian_chroot)}\[\033[95m\]\A\[\033[00m\]-\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[96m\]\w\[\033[00m\]\$'*


**Amaury QUERON et Solène SINNIG**


























































