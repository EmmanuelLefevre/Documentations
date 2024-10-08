# LINUX
## INTRODUCTION
Système d'exploitation open source de type Unix fondé sur le noyau Linux créé en 1991 par Linus Torvalds
## COMMANDS
### <= Vérification de version =>
| Command + option | Description |
| :--------------: | :---------: |
|`cat /etc/os-release`| Affiche les informations sur la distribution| `cat /etc/os-release`|
|`uname -a`| Affiche des informations sur le noyau| `uname -a`|
|`lsb_release -a`| Affiche les informations sur la version LSB| `lsb_release -a`|
|`dpkg --version`| Affiche la version de dpkg| `dpkg --version`|
### <= Réseau =>
| Command + option | Description |
| :--------------: | :---------: |
| `ifconfig 'eth0'`| Affiche la configuration réseau ethernet|
| `ifconfig 'wlan0'`| Affiche la configuration réseau wifi|
| `ping 'adresse_IP_ou_nom_de_domaine'`|Vérifie la connectivité réseau|
| `traceroute 'adresse_IP_ou_nom_de_domaine'`|Affiche le chemin des paquets réseau|
| `netstat -tuln`|Affiche les connexions réseau|
| `curl -o 'url_fichier`|Télécharger fichier en curl à partir d'une URL|
### <= Redémarrage / arrêt =>
| Command + option | Description |
| :--------------: | :---------: |
| `shutdown`     | Arrête l'instance                     | `shutdown [option]`          |
| `reboot`       | Redémarre l'instance                  | `reboot [option]`            |
| `poweroff`     | Éteint l'instance                     | `poweroff [option]`          |
| `halt`         | Arrête le système immédiatement       | `halt [option]`              |
### <= Navigation fichiers =>
| Command + option | Description |
| :--------------: | :---------: |
|` /`|Racine du disque|
|`cd ~ / cd $HOME`|Répertoire utilisateur|
|`cd var/www/`|Répertoire /var/www|
|`cd ..`|Répertoire parent|
|`cd ../../..`|Naviguer plusieurs niveaux en amont|
|`cd -`|Répertoire précédent|
### <= Affichage =>
| Command + option | Description |
| :--------------: | :---------: |
|`ls -l`|Informations et détails|
|`ls -a`|Fichiers cachés|
|`ls -h`|Poids fichiers plus lisible|
|`ls -r`|Tri inversé|
|`ls -t`|Tri par date récent -> ancien|
|`ls -S`|Tri par taille décroissante|
|`pwd`|Renvoyer chemin absolu du répertoire courant|
### <= Création répertoire =>
| Command + option | Description |
| :--------------: | :---------: |
|`mkdir 'foo'`|Créer répertoire 'foo'|
|`mkdir -v 'foo' '/tmp/bar'`|Créer répertoires 'foo' et '/tmp/bar'|
|`mkdir -p 'foo/bar/baz'`|Créer l’arborescence 'foo/bar/baz'|
|`mkdir -m 755 'new_folder'`|Créer répertoire 'new_folder' avec permissions 755|
|`mkdir -v`|Retourner des informations lors de la création d'un répertoire|
### <= Création fichier =>
| Command + option | Objectif |
| :--------------: | :---------: |
|`touch 'file.txt'`|Créer fichier vide nommé 'file.txt' (ou MAJ d'horodatage s'il existe déjà)|
|`touch 'file1.txt' 'file2.txt'`|Créer 'file1.txt' et 'file2.txt'|
|`touch -c 'file.txt'`|Ne pas créer de fichier si 'file.txt' n'existe pas, juste MAJ d'horodatage s'il existe|
|`touch -a 'file.txt'`|MAJ d'horodatage d'accès du fichier 'file.txt'|
|`touch -m 'file.txt'`|MAJ d'horodatage de modification du fichier 'file.txt'|
### <= Copier =>
| Command + option | Description |
| :--------------: | :---------: |
|`cp foo/bar.txt info/`|Copier 'bar.txt' situé dans le répertoire 'foo' vers le répertoire 'info'|
|`cp -r foo/ info/`|Copier répertoire 'foo' + son contenu dans le répertoire 'info' (si 'info' existe le contenu sera placé dans 'info/foo/')|
### <= Déplacer / Renommer =>
| Command + option | Description |
| :--------------: | :---------: |
|`mv 'project_v1/' 'project_v2/'`|Renommer répertoire 'project_v1' en 'project_v2'|
|`mv foo_bar.txt foo_stop.txt`|Renommer foo_bar.txt' en 'foo_stop.txt' dans le même répertoire|
|`mv foo/bar.txt info/`|Déplacer 'bar.txt' situé dans le répertoire 'foo' vers le répertoire 'info'|
|`mv temp.txt '/backup/'`|Déplacer 'temp.txt' vers le répertoire 'backup'|
|`mv '*.txt' '/documents/'`|Déplacer tous les '.txt' du répertoire courant vers le répertoire 'documents'|
### <= Effacer =>
| Command + option | Description |
| :--------------: | :---------: |
|`rm '*.txt'`|Supprimer tous les fichiers ayant pour extension 'txt'|
|`rm 'foo.txt' 'bar.txt'`|Supprimer les fichiers 'foo.txt' et 'bar.txt'|
|`rm -rf 'baz/'`|Supprimer le répertoire 'baz' et tout son contenu|
|`rm -i 'file.txt'`|Supprimer 'file.txt' après confirmation de l'utilisateur|
|`rm -v '*.log'`|Supprimer les fichiers ayant pour extension '.log' + afficher les noms des fichiers supprimés|
### <= Afficher contenu fichier =>
| Command + option | Description |
| :--------------: | :---------: |
|`cat 'file.txt'`|Afficher contenu 'file.txt' dans le terminal|
|`cat 'file1.txt' 'file2.txt'`|Afficher contenu de 'file1.txt' et 'file2.txt' en séquence|
|`cat > 'newfile.txt'`|Créer 'newfile.txt' et y entrer du texte, terminé par Ctrl+D|
|`cat -n 'file.txt'`|Afficher contenu de 'file.txt' avec les numéros de ligne|
|`cat >> 'existingfile.txt'`|Ajouter du texte à la fin de 'existingfile.txt', terminé par Ctrl+D|
### <= Trouver =>
| Command + option | Description |
| :--------------: | :---------: |
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
| Command + option | Description |
| :--------------: | :---------: |
|`grep 'text' foo.txt`|Occurences 'text' dans 'foo.txt'|
|`grep -v 'text' foo.txt`|Afficher les lignes de 'foo.txt' ne contenant pas l'occurence "text"|
|`grep -c 'text' foo.txt`|Compter nombre de lignes dans 'foo.txt' contenant l'occurence "text"'|
|`grep -n 'text' foo.txt`|Afficher les lignes de 'foo.txt' contenant "text", préfixées par leur numéro de ligne|
|`grep -x 'exact' foo.txt`|Afficher uniquement les lignes de 'foo.txt' qui correspondent exactement à la chaîne "exact"|
|`grep -l 'text' *.txt`|Afficher les noms des fichiers '.txt' dans le répertoire courant contenant l'occurrence "text"|
|`grep -r "text" /folderPath`|Rechercher de manière récursive l'occurence "text" dans folderPath|
|`grep -nri 'text' /project`|Recherche récursive, insensible à la casse, des occurrences de "text" dans répertoire 'project'|
|`grep -nri '\(foo\|bar\|baz\)' /project`|Recherche (récursive/insensible à la casse) occurrences de "foo""bar""baz" dans répertoire 'project'|
### <= Install packages =>
| Command + option | Description |
| :--------------: | :---------: |
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
| Command + option | Description |
| :--------------: | :---------: |
|`chmod u+w foo.txt`|Ajouter droits d'écriture au propriétaire (user, write)|
|`chmod g+r foo.txt`|Ajouter droits de lecture au groupe du fichier (group, read)|
|`chmod o-x foo.txt`|Supprimer droits d'exécution aux autres utilisateurs|
|`chmod a+rw projet`|Ajouter droits de lecture et d'écriture à tous|
|`chmod -R a+rx projet`|Ajouter droits de lecture/éxécution à tout le répertoire 'projet'|
|`chmod 764 dossier`|Propriétaire (7xx), lecture/écriture pour groupe (6xx), lecture uniquement pour autres (4xx)|
|`chmod -R 755 dossier`|Propriétaire tous droits (7xx) + droits lecture/accès aux autres (55) + récursif|
|`chmod 600 fichier`|Tous droits propriétaire (lecture/écriture), aucun droit pour le groupe et les autres|
|`chmod 644 fichier`|Droits lecture/écriture pour propriétaire, droits de lecture pour le groupe et les autres|
|`chmod +x script.sh`|Ajouter droit d'exécution à 'script.sh'|
|`chmod -R g+w projet`|Ajouter droits d'écriture au groupe pour répertoire 'projet' et tous ses contenus|
|`chmod u-s fichier`|Supprimer le bit setuid du fichier (ne pas exécuter fichier avec privilèges du propriétaire)|
#### Correspondances de représentation des droits
| Droit                                               | Valeur alphanumérique | Valeur octale | Description                                      |
| :-------------------------------------------------: | :-------------------: | :-----------: | :----------------------------------------------: |
| Aucun droit                                         | ---                   | 0             |                                                  |
| Exécution seulement                                 | --x                   | 1             |                                                  |
| Ecriture seulement                                  | -w-                   | 2             |                                                  |
| Ecriture et exécution                               | -wx                   | 3             |                                                  |
| Lecture seulement                                   | r--                   | 4             |                                                  |
| Lecture et exécution                                | r-x                   | 5             |                                                  |
| Lecture et écriture                                 | rw-                   | 6             |                                                  |
| Tous les droits (lecture, écriture et exécution)    | rwx                   | 7             |                                                  |
| Setuid                                              | rws                   | 4xx           | Exécution avec les privilèges du propriétaire    |
| Setgid                                              | rwx                   | 2xx           | Exécution avec les privilèges du groupe          |
| Sticky bit                                          | rwx+t                 | 1xx           | Fichiers dans un répertoire pouvant être supprimés uniquement par leur propriétaire|
### <= Changer propriétaire fichier =>
| Command + option | Description |
| :--------------: | :---------: |
|`chown bob:admin foo.txt`|Attribuer l’utilisateur 'bob' + groupe 'admin' à 'foo.txt'|
|`chown alice file.txt`|Changer propriétaire 'file.txt' en 'alice'|
|`chown :users file.txt`|Changer groupe 'file.txt' en 'users' sans modifier le propriétaire|
|`chown -R bob:admin /path/to/directory`|Attribuer 'bob' + groupe 'admin' à tous les fichiers et dossiers dans '/path/to/directory' de manière récursive|
|`chown --from=currentuser:newgroup file.txt`|Changer le propriétaire de 'file.txt' uniquement si l'utilisateur actuel est 'current'|
### <= SSH =>
#### Connection SSH à une machine distante!
| Command + option | Description |
| :--------------: | :---------: |
|`ssh john@remotehost.example.com`|Connexion machine distante login 'john'|
|`ssh-keygen -t dsa`|Génération clé DSA (à faire sur la machine locale)|
|`ssh-copy-id -i ~/.ssh/id_dsa.pub john@remotehost.example.com`|Copie clé publique sur la machine distante pour utilisateur 'john'|
|`ssh -p 2222 john@remotehost.example.com`|Connexion à la machine distante sur le port 2222|
|`ssh -i ~/.ssh/my_key john@remotehost.example.com`|Utiliser une clé privée spécifique pour la connexion à la machine distante|
|`ssh -v john@remotehost.example.com`|Activer le mode verbeux pour afficher des informations de débogage lors de la connexion|
|`ssh user@remotehost.example.com 'command'`|Exécuter une commande spécifique sur la machine distante sans ouvrir une session interactive|
|`ssh -D 8080 john@remotehost.example.com`|Configurer un tunnel SOCKS pour la redirection de trafic via la machine distante|
### <= SCP =>
#### Copier des fichiers entre le serveur et le client SSH de manière sécurisée!
| Command + option | Description |
| :--------------: | :---------: |
|`scp foo.txt john@remotehost.example.com:`|Transférer 'foo.txt' situé dans répertoire courant vers répertoire 'home' du compte 'john' de la machine 'remotehost.example.com'|
|`scp john@remotehost.example.com:foo.txt ./`|Récupèrer 'foo.txt' situé dans répertoire 'home' du compte 'john' pour le copier dans le répertoire courant|
|`scp john@remotehost.example.com:/backups/*.sql backups/`|Récupérer les fichiers '.sql' situés dans le répertoire '/backups' pour les copier dans le sous-répertoire 'backups' du répertoire courant|
|`scp -P 17654 john@remotehost:/files/ files/`|Récupérer les fichiers via un autre port (17654) que le port par défaut (22)|
|`scp -r mails/ john@remotehost:`|Transférer l'intégralité du répertoire 'mails' vers le répertoire 'home' du compte 'john' sur la machine distante|
|`scp -v foo.txt john@remotehost.example.com:`|Transférer 'foo.txt' avec un affichage verbeux pour le débogage|
|`scp -C foo.txt john@remotehost.example.com:`|Transférer 'foo.txt' avec compression pour réduire la taille des données transférées|
|`scp -i ~/.ssh/my_key foo.txt john@remotehost.example.com:`|Utiliser une clé privée spécifique pour l'authentification lors du transfert de 'foo.txt'|
|`scp -P 2222 john@remotehost:/path/to/file.txt ./`|Transférer un fichier depuis une machine distante en utilisant un port spécifique (ici 2222)|
|`scp -r john@remotehost:/path/to/directory ./`|Récupérer l'intégralité d'un répertoire depuis la machine distante vers le répertoire courant|
### <= Espace disque =>
| Command + option | Description |
| :--------------: | :---------: |
|`du -sh dossier1 dossier2`|Connaitre espace disque utilisé par les répertoires 'dossier1' et 'dossier2'|
|`du -hc --max-depth=1`|Afficher espace disque utilisé des fichiers et répertoires contenus dans le répertoire courant|
|`df -h`|Afficher espace disque disponible sur toutes les partitions (disk free)|
|`du -ah`|Afficher espace disque utilisé par tous les fichiers et répertoires + fichiers cachés|
|`du -sh *`|Afficher espace disque utilisé par chaque élément du répertoire courant|
|`df -i`|Afficher l'utilisation des inodes sur toutes les partitions|
|`df -h /mount_point`|Afficher l'espace disque disponible pour un point de montage spécifique|
|`du -s /path/to/directory`|Afficher l'espace disque utilisé par le répertoire spécifié sans afficher les sous-répertoires|
### <= ProcessuMémoires =>
| Command + option | Description |
| :--------------: | :---------: |
|`free`|Afficher la mémoire libre et utilisée sur le système|
### <= Processus =>
| Command + option | Description |
| :--------------: | :---------: |
|`top`|Classement en temps réel des processus en cours triés par utilisation du CPU ou de la mémoire|
|`ps aux`|Afficher tous les processus exécutés par tous les utilisateurs|
|`ps faux`|Afficher tous les processus exécutés sous forme d'arbre pour visualiser les relations entre eux|
|`kill pid`|Arrêter un processus spécifié par son identifiant (PID)|
|`kill -9 pid`|Tuer violemment le processus spécifié (déconseillé, à utiliser en dernier recours)|
|`htop`|Interface améliorée de `top` avec une interface utilisateur plus conviviale pour la gestion des processus|
|`pkill name`|Tuer tous les processus correspondant au nom spécifié|
|`killall name`|Tuer tous les processus par leur nom (similaire à `pkill`)|
|`vmstat`|Afficher des statistiques sur la mémoire virtuelle et l'utilisation du système|
|`lsof`|Lister tous les fichiers ouverts par les processus, utile pour diagnostiquer les problèmes de fichiers|
### <= Archives =>
#### TAR
| Command + option | Description |
| :--------------: | :---------: |
|`tar -cvf 'archive.tar' 'mon_dossier/'`|Créer une archive|
|`tar -tvf 'archive.tar'`| Tester / lister le contenu d'une archive|
|`tar -xvf 'archive.tar'`| Extraire les fichiers d'une archive|
|`tar -cvf 'archive.tar' 'mon_dossier/'`|Afficher des informations détaillées sur les fichiers archivés ou extraits|
|`tar -cvjf 'archive.tar.bz2' 'mon_dossier/'`|Utiliser le format de compression bzip2|
|`tar -czvf 'archive.tar.gz' 'mon_dossier/'`|Utiliser le format de compression gzip|
|`tar -cvf archive.tar foo.txt`|Créer archive nommée 'archive.tar' contenant 'foo.txt'|
|`tar -cvf archive.tar foo.txt foo2.txt`|Créer archive contenant les fichiers 'foo.txt' et 'foo2.txt'|
|`tar -cvf archive.tar projet/`|Créer archive à partir du répertoire 'projet/'|
|`tar -czvf archive.tar.gz projet/`|Créer archive au format tar.gz à partir du répertoire 'projet/'|
|`tar -cjvf archive.tar.bz2 projet/`|Créer archive au format tar.bz2 à partir du répertoire 'projet/'|
|`tar -xzvf archive.tar.gz`|Extraire archive 'archive.tar.gz'|
|`tar -xjvf archive.tar.bz2`|Extraire archive 'archive.tar.bz2'|
|`tar -tf mon_fichier.tar`|Lister les fichiers contenus dans l'archive 'mon_fichier.tar'|
|`tar --exclude='*.log' -cvf archive.tar projet/`|Créer archive tout en excluant les fichiers avec l'extension '.log'|
|`tar -cvf - fichier1 \| gzip > fichier1.tar.gz`|Créer archive tar compressée au format gzip en une seule commande|
|`tar -xvf archive.tar -C /destination/`|Extraire les fichiers de 'archive.tar' dans le répertoire spécifié '/destination/'|
#### GZIP
| Command + option | Description |
| :--------------: | :---------: |
|`gzip 'fichier.txt'`|Compresser un fichier en utilisant gzip|
|`gzip -d 'fichier.txt.gz'`|Décompresser un fichier compressé avec gzip|
|`gzip -k 'fichier.txt'`|Compresser un fichier tout en conservant l'original|
|`gzip -l 'fichier.txt.gz'`|Lister les informations sur le fichier compressé|
|`gzip -r 'dossier/'`|Compresser tous les fichiers d'un répertoire|
|`gzip -v 'fichier.txt'`|Afficher les détails de la compression|
|`gzip -1 'fichier.txt'`|Compresser un fichier avec la compression la plus rapide|
|`gzip -9 'fichier.txt'`|Compresser un fichier avec la meilleure compression|
|`gzip -c 'fichier.txt' > 'fichier.txt.gz'`|Compresser et rediriger la sortie vers un nouveau fichier|
|`gunzip 'fichier.txt.gz'`|Commande alternative pour décompresser un fichier gzip|
### UTILISATEURS
| Command + option | Description |
| :--------------: | :---------: |
|`sudo adduser 'nom_utilisateur'`|Ajouter un nouvel utilisateur|
|`sudo passwd 'nom_utilisateur'`|Changer le mot de passe d'un utilisateur|
### LOGS
| Command + option | Description |
| :--------------: | :---------: |
|`tail -f '/var/log/syslog'`|Afficher les logs en temps réel|
|`grep 'terme_de_recherche' '/var/log/syslog'`|Rechercher dans les logs|
### ENVIRONNEMENT
| Command + option | Description |
| :--------------: | :---------: |
|`export 'NOM_VARIABLE=valeur'`|Définir une variable d'environnement|
|`printenv`|Afficher les variables d'environnement|
