# POWERSHELL
## Introduction
Windows PowerShell (anciennement Microsoft Command Shell (MSH) nom de code Monad) est une suite logicielle développée par Microsoft qui intègre une interface en ligne de commande, un langage de script nommé PowerShell ainsi qu'un kit de développement.
## COMMANDES UTILES
### Lancer l'invite de commande
| Command          | Description          |
| :--------------: | :------------------: |
|`WIN + R`| Ouvrir la boîte de dialogue Exécuter|
|`cmd /k <commande>`|Lancer cmd avec une commande spécifique|
### Nouveau terminal admin depuis le Powershell
| Command          | Description          |
| :--------------: | :------------------: |
|`Start-Process powershell -Verb runAs`|Nouveau terminal en mode admin|
### Informations système
| Command          | Description          |
| :--------------: | :------------------: |
|`systeminfo`|Afficher des informations sur le système|
### Afficher les adresses IP
| Command          | Description          |
| :--------------: | :------------------: |
|`ipconfig`|Paramètres de connexion réseau de base|
|`ipconfig /all`|Afficher toutes les adresses IP (IPv4)|
|`ipconfig /release`|Libérer l'adresse IP actuelle (DHCP)|
|`ipconfig /renew`|Renouveler l'adresse IP actuelle (DHCP)|
### Diagnostique réseau
| Command          | Description          |
| :--------------: | :------------------: |
|`ping <adresse>`|Tester la connectivité réseau à une adresse IP ou un nom de domaine|
|`tracert <adresse>`|Afficher le chemin suivi par les paquets pour atteindre une adresse IP|
|`nslookup <nom_de_domaine>`|Résoudre le nom de domaine en adresse IP|
|`netstat -an`|Afficher toutes les connexions réseau et ports à l'écoute|
|`telnet <adresse> <port>`|Se connecter à un hôte distant sur un port spécifique|
|`telnet smtp.example.com 25`|Tester serveur SMTP sur l'adresse smtp.example.com sur le port 25|
|`telnet localhost 465`|Tester serveur SMTP localhost sur le port 465|
### Gestion du cache DNS
| Command          | Description          |
| :--------------: | :------------------: |
|`ipconfig /displaydns > logdns.txt`|Afficher le cache DNS dans un fichier logdns.txt|
|`ipconfig /flushdns`|Effacer le cache DNS|
### Processus
| Command          | Description          |
| :--------------: | :------------------: |
|`tasklist`|Afficher tous les processus en cours|
|`taskkill /IM <nom_du_processus>`|Terminer un processus spécifique|
### Gestion des services windows
| Command          | Description          |
| :--------------: | :------------------: |
|`services.msc`|Ouvrir la fenêtre des services Windows|
|`net start <nom_du_service>`|Démarrer un service Windows|
|`net stop <nom_du_service>`|Arrêter un service Windows|
|`net start`|Afficher les connexions réseau et les ports d'écoute|
### Gestion des fichiers et dossiers
| Command          | Description          |
| :--------------: | :------------------: |
|`mkdir <nom_dossier>`|Créer un nouveau dossier|
|`rmdir <nom_dossier>`|Supprimer un dossier|
|`del <nom_fichier>`|Supprimer un fichier|
|`copy <source> <destination>`|Copier un fichier|
|`move <source> <destination>`|Déplacer un fichier|
### Gestion de l'arrêt de l'ordinateur
| Command          | Description          |
| :--------------: | :------------------: |
|`shutdown -s -t 3600`|Arrêter l'ordinateur dans 1 heure|
|`shutdown -s -t 7200`|Arrêter l'ordinateur dans 2 heures|
|`shutdown -a`|Annuler l'arrêt|
|`shutdown -s -c "text"`|Arrêter avec un message (127 caractères maxi)|
|`shutdown -r`|Redémarrer l'ordinateur|

