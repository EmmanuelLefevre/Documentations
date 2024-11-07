# POWERSHELL

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [COMMANDES UTILES](#commandes-utiles)
  - [Lancer l'invite de commande](#lancer-linvite-de-commande)
  - [Informations système](#informations-système)
  - [Gestion des processus](#gestion-des-processus)
- [GESTION DES UTILISATEURS](#gestion-des-utilisateurs)
- [GESTION DES FICHIERS ET DES DOSSIERS](#gestion-des-fichiers-et-dossiers)
- [AFFICHER LES ADRESSES IP](#afficher-les-adresses-ip)
- [GESTION DU RESEAU](#gestion-du-reseau)
  - [Diagnostique réseau](#diagnostique-réseau)
  - [Gestion du cache DNS](#gestion-du-cache-dns)
  - [Processus](#processus)
  - [Gestion des services](#gestion-des-services)
- [MAJ WINDOWS](#maj-windows)
- [FONCTIONNALITES WINDOWS](#fonctionnalites-windows)
- [CONNEXION A UN ORDINATEUR DISTANT](#connexion-a-un-ordinateur-distant)
- [GESTION D'ARRET DE L'ORDINATEUR](#gestion-darret-de-lordinateur)

## INTRODUCTION
Windows PowerShell (anciennement Microsoft Command Shell (MSH) nom de code Monad) est une suite logicielle développée par Microsoft qui intègre une interface en ligne de commande, un langage de script nommé PowerShell ainsi qu'un kit de développement.

## COMMANDES UTILES
### Lancer l'invite de commande
| Command          | Description          |
| :--------------: | :------------------: |
|`WIN + R`| Ouvrir la boîte de dialogue Exécuter|
|`cmd /k <commande>`|Lancer cmd avec une commande spécifique|

### Informations système
| Command          | Description          |
| :--------------: | :------------------: |
|`systeminfo`|Afficher des informations sur le système|

### Gestion des processus
| Command          | Description          |
| :--------------: | :------------------: |
|`Start-Process powershell -Verb runAs`|Nouveau terminal en mode admin|
|`Get-Process`|Lister les processus en cours d'exécution|
|`Stop-Process -Name <processus>`|Arrêter un processus|
|`Start-Process <processus>`|Démarrer un nouveau processus|
|`Wait-Process -Name <processus>`|Attendre qu'un processus se termine|

## GESTION DES UTILISATEURS
| Commande | Description |
| :---: | :---: |
|`Get-LocalUser`|Lister les utilisateurs locaux|
|`New-LocalUser <utilisateur>`|Créer un nouvel utilisateur local|
|`Remove-LocalUser <utilisateur>`|Supprimer un utilisateur local|
|`Set-LocalUser <utilisateur> -Password <motdepasse>`|Définir le mot de passe d'un utilisateur local|
|`Add-LocalGroupMember -Group Administrators -Member <utilisateur>`|Ajouter un utilisateur au groupe Administrateurs|
|`Remove-LocalGroupMember -Group Administrators -Member <utilisateur>`|Supprimer un utilisateur du groupe Administrateurs|

## GESTION DES FICHIERS ET DES DOSSIERS
| Commande | Description |
| :---: | :---: |
|`Get-ChildItem`|Lister les fichiers et répertoires|
|`Get-Content <fichier>`|Obtenir le contenu d'un fichier|
|`Set-Content <fichier> <contenu>`|Définir le contenu d'un fichier|
|`New-Item <fichier>`|Créer un nouveau fichier|
|`New-Item <répertoire> -ItemType Directory`|Créer un nouveau répertoire|
|`Remove-Item <fichier>`|Supprimer un fichier|
|`Remove-Item <répertoire> -Recurse`|Supprimer un répertoire|
|`Rename-Item <fichier> <nouveau_fichier>`|Renommer un fichier ou un répertoire|
|`Copy-Item SOURCE DEST`|Copier un fichier|
|`Copy-Item SOURCE DEST -Recurse`|Copier un répertoire|
|`Move-Item SOURCE DEST`|Déplacer un fichier ou un répertoire|

## AFFICHER LES ADRESSES IP
| Command          | Description          |
| :--------------: | :------------------: |
|`ipconfig`|Paramètres de connexion réseau de base|
|`ipconfig /all`|Afficher toutes les adresses IP (IPv4)|
|`ipconfig /release`|Libérer l'adresse IP actuelle (DHCP)|
|`ipconfig /renew`|Renouveler l'adresse IP actuelle (DHCP)|

## GESTION DU RESEAU
| Commande | Description |
| :---: | :---: |
|`Get-NetIPAddress`|Lister les adresses IP|
|`Get-NetAdapter`|Lister les adaptateurs réseau|

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

### Gestion des services
| Command          | Description          |
| :--------------: | :------------------: |
|`Get-Service`|Lister les services|
|`Start-Service <service>`|Démarrer un service|
|`Stop-Service <service>`|Arrêter un service|
|`Restart-Service <service>`|Redémarrer un service|
|`Set-Service <service> -StartupType Automatic`|Configurer un service pour qu'il démarre automatiquement|
|`Set-Service <service> -StartupType Manual`|Configurer un service pour qu'il démarre manuellement|
|`Set-Service <service> -StartupType Disabled`|Désactiver un service|
|`services.msc`|Ouvrir la fenêtre des services Windows|
|`net start <nom_du_service>`|Démarrer un service Windows|
|`net stop <nom_du_service>`|Arrêter un service Windows|
|`net start`|Afficher les connexions réseau et les ports d'écoute|

## MAJ WINDOWS
| Commande | Description |
| :---: | :---: |
|`Install-Module -Name PSWindowsUpdate`|Installer le module PSWindowsUpdate|
|`Get-Command -Module PSWindowsUpdate`|Lister toutes les commandes dans le module PSWindowsUpdate|
|`Get-WUInstall`|Installer les mises à jour Windows|

## FONCTIONNALITES WINDOWS
| Commande | Description |
| :---: | :---: |
|`Get-WindowsFeature`|Lister les fonctionnalités Windows|
|`Install-WindowsFeature <fonctionnalité>`|Installer une fonctionnalité Windows|
|`Uninstall-WindowsFeature <fonctionnalité>`|Désinstaller une fonctionnalité Windows|

## CONNEXION A UN ORDINATEUR DISTANT
| Commande | Description |
| :---: | :---: |
|`Enter-PSSession -ComputerName <nom> -Credential <utilisateur>`|Ouvrir une nouvelle session distante|
|`Exit-PSSession`|Fermer la session distante en cours|
|`Invoke-Command -ComputerName <nom> -ScriptBlock { <commande> }`|Exécuter une commande sur un ordinateur distant|
|`Invoke-Command -ComputerName <nom> -FilePath <script>`|Exécuter un script sur un ordinateur distant|

## GESTION D'ARRET DE L'ORDINATEUR
| Command          | Description          |
| :--------------: | :------------------: |
|`shutdown -s -t 3600`|Arrêter l'ordinateur dans 1 heure|
|`shutdown -s -t 7200`|Arrêter l'ordinateur dans 2 heures|
|`shutdown -a`|Annuler l'arrêt|
|`shutdown -s -c "text"`|Arrêter avec un message (127 caractères maxi)|
|`shutdown -r`|Redémarrer l'ordinateur|

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
