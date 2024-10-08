# Linux
## INTRODUCTION
Système d'exploitation open source de type Unix fondé sur le noyau Linux créé en 1991 par Linus Torvalds
## COMMANDS
### <= Navigation fichiers =>
| Command + option | Objectif |
| :---------: | :---------: |
|` /`|Racine du disque|
|`cd ~ / cd $HOME`|Répertoire utilisateur|
|`cd var/www/`|Répertoire /var/www|
|`cd ..`|Répertoire parent|
|`cd -`|Répertoire précédent|
### <= Affichage =>
| Command + option | Objectif |
| :---------: | :---------: |
|`ls -l`|Informations et détails|
|`ls -a`|Fichiers cachés|
|`ls -h`|Poids fichiers plus lisible|
|`ls -r`|Tri inversé|
|`ls -t`|Tri par date récent -> ancien|
|`ls -S`|Tri par taille décroissante|
|`pwd`|Renvoyer chemin absolu du répertoire courant|
### <= Création répertoire =>
| Command + option | Objectif |
| :---------: | :---------: |
|`mkdir 'foo'`|Créer répertoire 'foo'|
|`mkdir -v 'foo' '/tmp/bar'`|Créer répertoires 'foo' et '/tmp/bar'|
|`mkdir -p 'foo/bar/baz'`|Créer l’arborescence 'foo/bar/baz'|
|`mkdir -m 755 'new_folder'`|Créer répertoire 'new_folder' avec permissions 755|
|`mkdir -v`|Retourner des informations lors de la création d'un répertoire|
### <= Création fichier =>
| Command + option | Objectif |
| :---------: | :---------: |
|`touch 'file.txt'`|Créer fichier vide nommé 'file.txt' (ou MAJ d'horodatage s'il existe déjà)|
|`touch 'file1.txt' 'file2.txt'`|Créer 'file1.txt' et 'file2.txt'|
|`touch -c 'file.txt'`|Ne pas créer de fichier si 'file.txt' n'existe pas, juste MAJ d'horodatage s'il existe|
|`touch -a 'file.txt'`|MAJ d'horodatage d'accès du fichier 'file.txt'|
|`touch -m 'file.txt'`|MAJ d'horodatage de modification du fichier 'file.txt'|
### <= Copier =>
| Command + option | Objectif |
| :---------: | :---------: |
|`cp foo/bar.txt info/`|Copier 'bar.txt' situé dans le répertoire 'foo' vers le répertoire 'info'|
|`cp -r foo/ info/`|Copier répertoire 'foo' + son contenu dans le répertoire 'info' (si 'info' existe le contenu sera placé dans 'info/foo/')|
### <= Déplacer / Renommer =>
| Command + option | Objectif |
| :---------: | :---------: |
|`mv 'project_v1/' 'project_v2/'`|Renommer répertoire 'project_v1' en 'project_v2'|
|`mv foo_bar.txt foo_stop.txt`|Renommer foo_bar.txt' en 'foo_stop.txt' dans le même répertoire|
|`mv foo/bar.txt info/`|Déplacer 'bar.txt' situé dans le répertoire 'foo' vers le répertoire 'info'|
|`mv temp.txt '/backup/'`|Déplacer 'temp.txt' vers le répertoire 'backup'|
|`mv '*.txt' '/documents/'`|Déplacer tous les '.txt' du répertoire courant vers le répertoire 'documents'|
### <= Effacer =>
| Command + option | Objectif |
| :---------: | :---------: |
|`rm '*.txt'`|Supprimer tous les fichiers ayant pour extension 'txt'|
|`rm 'foo.txt' 'bar.txt'`|Supprimer les fichiers 'foo.txt' et 'bar.txt'|
|`rm -rf 'baz/'`|Supprimer le répertoire 'baz' et tout son contenu|
|`rm -i 'file.txt'`|Supprimer 'file.txt' après confirmation de l'utilisateur|
|`rm -v '*.log'`|Supprimer les fichiers ayant pour extension '.log' + afficher les noms des fichiers supprimés|
### <= Afficher contenu fichier =>
| Command + option | Objectif |
| :---------: | :---------: |
|`cat 'file.txt'`|Afficher contenu 'file.txt' dans le terminal|
|`cat 'file1.txt' 'file2.txt'`|Afficher contenu de 'file1.txt' et 'file2.txt' en séquence|
|`cat > 'newfile.txt'`|Créer 'newfile.txt' et y entrer du texte, terminé par Ctrl+D|
|`cat -n 'file.txt'`|Afficher contenu de 'file.txt' avec les numéros de ligne|
|`cat >> 'existingfile.txt'`|Ajouter du texte à la fin de 'existingfile.txt', terminé par Ctrl+D|
### <= Trouver =>
| Command + option | Objectif |
| :---------: | :---------: |
|`find 'myfile*' -print`|Rechercher fichier commençant par 'myfile'|
|`find -name '*myfile*.txt' -print`|Rechercher fichier contenant 'myfile' + extension '.txt'|
|`find '/usr' -type d -print`|Afficher tous les répertoires de '/usr'|
|`find '$HOME' \( -name '*.txt' -o -name '*.pdf' \)`|Afficher tous les fichiers '.txt' ou '.pdf' dans répertoire $HOME|
|`find '$HOME' -name '*.txt' -atime +7 -exec rm {} \;`|Supprimer tous les fichiers '.txt' qui n'ont pas été consultés depuis plus de 7 jours dans répertoire $HOME|
|`find '$HOME' -name '*.txt' -size +4k -exec ls -l {} \;`|Afficher taille tous les fichiers de plus de 4 kilo dans répertoire $HOME|
|`find -name 'file.txt'`|Recherche fichier nommé 'file.txt'|
|`find -iname 'file.txt'`|Recherche fichier nommé 'file.txt', insensible à la casse|
|`find -type d`|Rechercher tous les répertoires (d) dans le répertoire courant|
|`find -type f`|Rechercher tous les fichiers (f) dans le répertoire courant|
|`find -atime +7`|Rechercher fichiers qui n'ont pas été accédés depuis plus de 7 jours|
|`find -mtime -5`|Rechercher fichiers modifiés dans les 5 derniers jours|
|`find -user 'username'`|Rechercher fichiers appartenant à l'utilisateur spécifié 'username'|
|`find -group 'groupname'`|Rechercher fichiers appartenant au groupe spécifié 'groupname'|
|`find -size +1M`|Rechercher fichiers dont la taille est supérieure à 1 mégaoctet|
|`find -exec rm {} \;`|Supprimer les fichiers trouvés par la commande `find`|
|`find -a`|Opérateur ET (exemple : `find -name '*.txt' -a -size +1k` pour trouver fichiers .txt de plus de 1 Ko)|
|`find -o`|Opérateur OU (exemple : `find -name '*.jpg' -o -name '*.png'` pour trouver fichiers .jpg ou .png)|
|`find ! -name 'file.txt'` ou `find -not -name 'file.txt'`|Rechercher tous les fichiers sauf 'file.txt'|
### <= Rechercher chaînes de caractères ou motif =>
| Command + option | Objectif |
| :---------: | :---------: |
|`grep 'text' foo.txt`|Occurences 'text' dans 'foo.txt'|
|`grep -v 'text' foo.txt`|Afficher les lignes de 'foo.txt' ne contenant pas l'occurence "text"|
|`grep -c 'text' foo.txt`|Compter nombre de lignes dans 'foo.txt' contenant l'occurence "text"'|
|`grep -n 'text' foo.txt`|Afficher les lignes de 'foo.txt' contenant "text", préfixées par leur numéro de ligne|
|`grep -x 'exact' foo.txt`|Afficher uniquement les lignes de 'foo.txt' qui correspondent exactement à la chaîne "exact"|
|`grep -l 'text' *.txt`|Afficher les noms des fichiers '.txt' dans le répertoire courant contenant l'occurrence "text"|
|`grep -r "text" /folderPath`|Rechercher de manière récursive l'occurence "text" dans folderPath|
|`grep -nri 'text' /project`|Recherche récursive, insensible à la casse, des occurrences de "text" dans le répertoire /project.|
|`grep -nri '\(foo\|bar\|baz\)' /project`|Recherche récursive, insensible à la casse, des occurrences de "foo", "bar" ou "baz" dans le répertoire /project|
### <= Install packages =>
| Command + option | Objectif |
| :---------: | :---------: |
|`apt-get update`|MAJ liste des fichiers disponibles dans les dépôts APT|
|`apt-get upgrade`|Installer les MAJ en dernière version disponible|
|`apt-get install samba`|Installer le paquet 'Samba'|
|`apt-get install foo=2.2-1`|Installer le paquet 'foo' dans sa version '2.2-1'|
|`apt-get remove samba`|Désinstaller le paquet 'Samba' tout en laissant les fichiers de configuration|
|`apt-get purge samba`|Supprimer complètement le paquet 'Samba' et ses fichiers de configuration|
|`apt-cache policy php5`|Récupérer des informations sur l'état du paquet 'php5'|
|`dpkg -l \| grep php`|Lister tous les paquets 'php' installés sur la machine|
|`apt-get dist-upgrade`|Mettre à jour les paquets, en gérant les dépendances et les changements de paquets|
|`apt-get autoremove`|Supprimer les paquets installés automatiquement qui ne sont plus nécessaires|
|`apt-cache search <package>`|Rechercher un paquet disponible dans les dépôts APT|
|`apt-get clean`|Supprimer les fichiers de paquets (.deb) téléchargés pour libérer de l'espace|
|`apt-get show <package>`|Afficher des informations détaillées sur un paquet spécifique|
|`dpkg -i <package.deb>`|Installer un paquet à partir d'un fichier .deb local|
|`dpkg --remove <package>`|Désinstaller un paquet sans supprimer les fichiers de configuration|
|`dpkg -S <file>`|Trouver quel paquet a installé un fichier donné sur le système|
|`apt-get reinstall <package>`|Réinstaller un paquet sans le supprimer au préalable|
### <= Changer droit d'un fichier =>
| Command + option | Objectif |
| :---------: | :---------: |
|`chmod u+w fichier`|Ajouter droits d'écriture au propriétaire (user, write)|
|`chmod g+r fichier`|Ajouter droits de lecture au groupe du fichier (group, read)|
|`chmod o-x fichier`|Supprimer droits d'exécution aux autres utilisateurs (other, execution)|
|`chmod a+rw dossier`|Ajouter droits de lecture et d'écriture à tous (all)|
|`chmod -R a+rx files`|Ajouter droits de lecture et d'exécution à tout ce que contient le répertoire 'files'|
|`chmod 764 dossier`|Tous droits pour le propriétaire (7xx), lecture et écriture pour le groupe (6xx), et lecture uniquement pour autres (4xx)|
|`chmod -R 755 dossier`|Donner au propriétaire tous droits (7xx), alors que seuls les droits de lecture et d'accès seront donnés aux autres (55), avec l'option -R pour appliquer ces droits à tous les fichiers et dossiers contenus dans 'dossier'|
|`chmod 600 fichier`|Tous droits pour le propriétaire (lecture et écriture), aucun droit pour le groupe et les autres|
|`chmod 644 fichier`|Droits lecture écriture pour le propriétaire, droits de lecture pour le groupe et les autres|
|`chmod +x script.sh`|Ajouter droit d'exécution au fichier script.sh|
|`chmod -R g+w dossier`|Ajouter droits d'écriture au groupe pour le répertoire 'dossier' et tous ses contenus|
|`chmod u-s fichier`|Supprimer le bit setuid du fichier (ne pas exécuter le fichier avec les privilèges du propriétaire)|
#### Correspondances de représentation des droits
| Droit                                               | Valeur alphanumérique | Valeur octale |
|-----------------------------------------------------|-----------------------|----------------|
| aucun droit                                         | ---                   | 0              |
| exécution seulement                                 | --x                   | 1              |
| écriture seulement                                   | -w-                   | 2              |
| écriture et exécution                               | -wx                   | 3              |
| lecture seulement                                    | r--                   | 4              |
| lecture et exécution                                | r-x                   | 5              |
| lecture et écriture                                  | rw-                   | 6              |
| tous les droits (lecture, écriture et exécution)    | rwx                   | 7              |

#### Correspondances de représentation des droits
| Droit                                               | Valeur alphanumérique | Valeur octale | Description                                      |
|-----------------------------------------------------|-----------------------|----------------|--------------------------------------------------|
| Aucun droit                                         | ---                   | 0              ||
| Exécution seulement                                 | --x                   | 1              ||
| Ecriture seulement                                   | -w-                   | 2              ||
| Ecriture et exécution                               | -wx                   | 3              ||
| Lecture seulement                                    | r--                   | 4              ||
| Lecture et exécution                                | r-x                   | 5              ||
| Lecture et écriture                                  | rw-                   | 6              ||
| Tous les droits (lecture, écriture et exécution)    | rwx                   | 7              ||
| Setuid                                              | rws                   | 4xx            | Exécution avec les privilèges du propriétaire    |
| Setgid                                              | rwx                   | 2xx            | Exécution avec les privilèges du groupe          |
| Sticky bit                                          | rwx+t                 | 1xx            | Fichiers dans un répertoire peuvent être supprimés uniquement par leur propriétaire |

### <= Changer propriétaire fichier =>
| Command + option | Objectif |
| :---------: | :---------: |
|`chown bob:admin foo.txt`|Attribuer l’utilisateur 'bob' + groupe 'admin' à 'foo.txt'|
|`chown alice file.txt`|Changer propriétaire du fichier 'file.txt' en 'alice'|
|`chown :users file.txt`|Changer groupe du fichier 'file.txt' en 'users' sans modifier le propriétaire|
|`chown -R bob:admin /path/to/directory`|Attribuer 'bob' + groupe 'admin' à tous les fichiers et dossiers dans '/path/to/directory' de manière récursive|
|`chown --from=currentuser:newgroup file.txt`|Changer le propriétaire de 'file.txt' uniquement si l'utilisateur actuel est 'current|
### <= SSH =>
| Command + option | Objectif |
| :---------: | :---------: |
### <= SCP =>
| Command + option | Objectif |
| :---------: | :---------: |
### <= Espace disque =>
| Command + option | Objectif |
| :---------: | :---------: |
### <= Processus =>
| Command + option | Objectif |
| :---------: | :---------: |
### <= Archives =>
| Command + option | Objectif |
| :---------: | :---------: |



         ---------
            SSH
         ---------

Se connecter de façon sécurisée à une machine distante!

ssh john@remotehost.example.com                                         Connexion à la machine distante avec le login john
ssh -l john remotehost.example.com                                      Equivaut à la commande précédente
ssh-keygen -t dsa                                                       Génération d'une clé DSA (à faire sur la machine locale)
ssh-copy-id -i ~/.ssh/id_dsa.pub john@remotehost.example.com            Copie de la clé publique sur la machine distante



         ---------
            SCP
         ---------

Permet de copier des fichiers entre le serveur et le client ssh de manière sécurisée!

scp foo.txt john@remotehost.example.com:                    Transfère le fichier foo.txt situé dans le répertoire courant vers le home du compte 
                                                            john de la machine remotehost.example.com

scp john@remotehost.example.com:foo.txt ./                  Récupère le fichier foo.txt situé dans le home du répertoire du compte john pour le 
                                                            copier dans le répertoire courant

scp john@remotehost.example.com:/backups/*.sql backups/     Récupérer les fichiers .sql situés dans le répertoire backups pour le copier dans le 
                                                            sous-répertoire backups

scp -P 17654 john@remotehost:/files/ files/                 Récupérer les fichiers via un autre port (17654) que le port par défaut (22)

scp -r mails/ john@remotehost:                              Transfère l'intégralité du répertoire mails



        -------------
        ESPACE DISQUE
        -------------

du -sh dossier1 dossier2               Connaitre l'espace disque utilisé des deux répertoires (disk usage)
du -hc --max-depth=1                   Afficher l'espace disque utilisé des fichiers et répertoires contenu dans un répertoire
df -h                                  Afficher l'espace disque disponible (disk free)


        ---------
        PROCESSUS
        ---------

top                 Classement en live des processus en cours triés par utilisation Proc, Mem ou Temps CPU
free                Afficher la mémoire libre
ps aux              Afficher tous les processus exécutés
ps faux             Afficher tous les processus exécutés affiché sous forme
kill pid            Arrêter un processus
kill ­9 pid          Tuer violemment le processus (déconseillé)



        --------
        ARCHIVES
        --------

Quelques options :
-c:           Créer
-t:           Tester / lister
-x:           Extraire
-v:           Description des fichiers désarchivés
-j:           Format de compression bzip2
-z:           Format de compression gzip


tar -cvf archive.tar fichier1                           Création d’une archive nommée archive.tar contenant le fichier fichier1
tar -cvf archive.tar fichier1 fichier2                  Création d’une archive contenant deux fichiers fichier1 et fichier2
tar -cvf archive.tar repertoire/                        Création d’une archive a partir d’un répertoire
tar -czvf archive.tar.gz repertoire/                    Création d’une archive au format tar.gz
tar -cjvf archive.tar.bz2 repertoire/                   Création d’une archive au format tar.bz2
tar -xzvf archive.tar.gz                                Extraction de l’archive tar.gz
tar -xjvf archive.tar.bz2                               Extraction de l’archive tar.bz2
tar -tf mon_fichier.tar                                 Liste tous les fichiers contenus dans une archive














(touche) insert              Coller du texte dans la console