# Linux
## INTRODUCTION
Système d'exploitation open source de type Unix fondé sur le noyau Linux créé en 1991 par Linus Torvalds
## COMMANDS
### <= Navigation fichiers =>
| Command + option | Objectif |
| :---------: | :---------: |
|cd /|Racine du disque|
|cd ~ / cd $HOME|Répertoire utilisateur|
|cd var/www/|Répertoire /var/www|
|cd ..|Répertoire parent|
|cd -|Répertoire précédent|
### <= Affichage =>
| Command + option | Objectif |
| :---------: | :---------: |
|ls -l|Informations et détails|
|ls -a|Fichiers cachés|
|ls -h|Poids fichiers plus lisible|
|ls -r|Tri inversé|
|ls -t|Tri par date récent -> ancien|
|ls -S|Tri par taille décroissante|
|pwd|Renvoyer chemin absolu du répertoire courant|



        ----------
        RECHERCHER
        ----------

grep permet de rechercher une chaîne de caractères ou un motif dans un fichier.

Quelques options :
-v:             Affiche les lignes ne contenant pas la chaîne.
-c:             Compte le nombre de lignes contenant la chaîne.
-n:             Retourne les lignes préfixées par leur numéro.
-x:             Ligne correspondant exactement à la chaîne.
-l:             Affiche le nom des fichiers qui contiennent la chaîne.


grep 'text' foo.txt                             Recherche l'occurence 'text' dans le fichier foo.txt
grep -nri 'foobar' /project                     Recherche toutes les occurences de 'foobar' dans le repertoire /project
grep -nri '\(foo\|bar\|baz\)' /project          Recherche toutes les occurences à 'foo', 'bar' et 'baz' dans le repertoire /project



        ------
        COPIER
        ------

cp foo/bar.txt info/            Copier le fichier bar.txt dans le répertoire info
cp -r foo/ info/                Copier des répertoires entiers (note : si info existe, la cible sera info/foo/)



        -------------------
        DEPLACER / RENOMMER
        -------------------

mv foo/bar.txt info/                    Déplacer le fichier bar.txt dans le répertoire info
mv foo_bar.txt foo_stop.txt             Renommer le fichier foo_bar.txt en foo_stop.txt



        -------
        EFFACER
        -------

rm *.txt                    Supprimer tous les fichiers ayant pour extension txt
rm foo.txt bar.txt          Supprimer les fichiers foo.txt et bar.txt
rm -rf baz/                 Supprimer le répertoire baz et tout son contenu



        ----------------
        CREER REPERTOIRE
        ----------------

mkdir foo                       Créer le répertoire foo
mkdir -v                        Retourner des informations lors de la création d'un répertoire    
mkdir -p                        Cette option permet de créer une arborescence complète
mkdir -v foo /tmp/bar           Créer les répertoires foo et /tmp/bar
mkdir -p foo/bar/baz            Créer l’arborescence foo/bar/baz



        ------------------
        PACKAGES / INSTALL
        ------------------

apt-get update                              Mettre à jour la liste des fichiers disponibles dans les dépôts APT
apt-get install samba                       Installer du paquet Samba
apt-get install foo=2.2-1                   Installer du paquet foo dans sa version 2.2-1
apt-get remove samba                        Désinstallation du paquet Samba tout en laissant les fichiers de configuration
apt-get purge samba                         Suppression complète du paquet Samba et de ses fichiers de configuration
apt-cache policy php5                       Récupération d'informations sur l'état du paquet php5
dpkg -l | grep php                          Lister tous les paquets php installés sur la machine


      
        ---------------------------------
        CHANGER PROPRIETAIRE D'UN FICHIER
        ---------------------------------

chown bob:admin foo.txt             Attribuer l’utilisateur bob et le groupe admin au fichier foo.txt



        ---------------------------------------------
        CHANGER LES DROITS D'UN FICHIER OU REPERTOIRE
        ---------------------------------------------

chmod u+w fichier               Ajouter les droits d'écriture au propriétaire (user, write)
chmod g+r fichier               Ajouter les droits de lecture au groupe du fichier (group, read)
chmod o-x fichier               Supprimer les droits d'exécution aux autres utilisateurs (other, execution)
chmod a+rw dossier              Ajouter les droits de lecture / écriture à tous (all)
chmod -R a+rx files             Ajouter les droits de lecture et d'exécution à tout ce que contient le repertoire dossier
chmod 764 dossier               Tous les droits pour le propriétaire (7xx), lecture et ecriture pour le groupe (x6x) et lecture uniquement pour les autres (xx4)
chmod -R 755 dossier            Donner au propriétaire tous les droits (7xx), alors que seuls les droits de lecture et d'accès seront donnés aux autres (x55),
                                Grace à l'option -R ces droits seront appliqués à tous les fichiers et dossiers contenus dans ce répertoire


                                    Correspondances de représentation des droits
                                    --------------------------------------------

                Droit	                              Valeur alphanumérique	             Valeur octale
            aucun droit	                                        ---	                            0
            exécution seulement	                                --x	                            1
            écriture seulement	                                -w-	                            2
            ecriture et exécution	                            -wx	                            3
            lecture seulement	                                r--	                            4
            lecture et exécution	                            r-x	                            5
            lecture et écriture	                                rw-	                            6
    tous les droits (lecture, écriture et exécution)	        rwx	                            7



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



        -------
        TROUVER
        -------

find permet de chercher des fichiers et éventuellement d'exécuter des commandes dessus.

Quelques options :
-name:              Recherche d'un fichier par son nom
-iname:             Même chose que -name mais insensible à la casse
-type:              Recherche de fichier d'un certain type
-atime:             Recherche par date de dernier accès
-mtime:             Recherche par date de dernière modification
-user:              Recherche de fichiers appartenant à l'utilisateur donné
-group:             Recherche de fichiers appartenant au groupe donné
-size:              Recherche par rapport à une taille de fichier.
-exec:              Exécute la commande donnée aux fichiers trouvés.
-a:                 Opérateur ET
-o:                 Opérateur OU
! ou -not:          Opérateur NOT


find myfile* -print                                         Rechercher un fichier commençant par "myfile"
find -name *myfile*.txt -print                              Rechercher un fichier contenant "myfile" et ayant pour extention ".txt"
find /usr -type d -print                                    Afficher tous les répertoires de /usr
find $HOME \( -name '*.txt' -o -name '*.pdf' \)             Afficher tous les fichiers .txt ou .pdf dans le répertoire home de l'utilisateur
find $HOME -name *.txt -atime +7 -exec rm {} \;             Supprimer tous les fichiers .txt qui n'ont pas été consultés depuis plus de 7 jours 
                                                            dans le répertoire home de l'utilisateur
find $HOME -name '*.txt' -size +4k -exec ls -l {} \;        Afficher la taille de tous les fichiers de plus de 4 kilos



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