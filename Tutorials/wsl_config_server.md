# CONFIGURATION SERVEUR SUR WSL
## INTRODUCTION
Cette documentation a pour objectif de guider l'utilisateur dans la configuration d'un environnement de serveur web sur Windows Subsystem for Linux (WSL) avec les technologies MariaDB et Apache2. Elle couvre les étapes essentielles, de la mise en place du serveur fonctionnel avec sa base de données et son projet web.
## COMMANDES WSL
### 1. Afficher les distributions installées
```shell
wsl -l -v
```
### 2. Définir la distribution utilisée
```shell
wsl --set-default Ubuntu-22.04
```
### 3. Configurer WSL2 pour toutes les distributions
```shell
wsl --set-default-version 2
```
### 4. Stopper la distribution
```shell
wsl --shutdown
```
## INSTALLATION DE MARIADB
### 1. Afficher les services de la distribution
```shell
sudo service --status-all
```
### 2. Installer Mariadb
```shell
sudo apt install mariadb-server
```
### 3. Démarrer Mariadb
```shell
sudo service mariadb start
```
### 4. Sécuriser l'installation
```shell
sudo mysql_secure_installation
```
## Création d'un utilisateur Administrateur
### 1. Se connecter à Mariadb
```shell
sudo mariadb
```
### 2. Créer un utilisateur admin
```shell
GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'nimda' WITH GRANT OPTION;
```
### 3. Mettre à jour les permissions
```shell
FLUSH PRIVILEGES;
```
### 4. Quitter Mariadb
```shell
exit
```
### 5. Vérifier si l'utilisateur admin a été crée
```shell
mysqladmin -u admin -p version
```
## Création d'un utilisateur Standard
### 1. Se connecter en admin
```shell
mysql -u admin -p
```
### 2. Afficher les bases de données
```shell
SHOW DATABASES;
```
### 3. Créer une base de données
```shell
CREATE DATABASE easygarden;
```
### 4. Créer un utilisateur
```shell
CREATE USER 'user'@'%' IDENTIFIED BY 'xx';
```
### 5. Afficher tous les utilisateurs
```shell
SELECT user FROM mysql.user;
```
### 6. Ajouter des droits à l'utilisateur sur une base de données
```shell
GRANT ALL PRIVILEGES ON easygarden.* TO 'user'@'%';
```
### 7. Mettre à jour les permissions
```shell
FLUSH PRIVILEGES;
```
### 8. Afficher liaison utilisateur/base de données + droits
```shell
SHOW GRANTS FOR 'user'@'%';
```
### 9. Se connecter en tant qu'utilisateur Standart
```shell
mysql -u user -p
```
## UTILISATION DE NETSTAT
### 1. Installer Netstat
```shell
sudo apt install net-tools
```
### 2. Afficher les connexions réseaux
```shell
sudo netstat -aln
```
## SE CONNECTER A LINUX DEPUIS WINDOWS AVEC HEIDISQL
### 1. Récupérer son ip
```shell
ip a | grep eth0
```
- Note : Utiliser la première IP sans le slash et ce qui suit.
### 2. Changer la config de Mariadb pour écouter depuis l'extérieur
```shell
cd /etc/mysql
```
```shell
sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf
```
### 3. Commenter la ligne bind-adress ou remplacer 127.0.0.1 par l'ip ethernet de windows

## INSTALLATION D'APACHE2
### 1. Installer Apache2
```shell
sudo apt install apache2
```
### 2. Vérifier le statut du service Apache2
```shell
sudo service apache2 status
```
### 3. Démarrer le service apache2
```shell
sudo service apache2 start
```
### 4. Afficher le hostname par défaut
```shell
hostname -I
```
### 5. Afficher liste des utilisateurs Apache
```shell
sudo cat /etc/passwd
```
### 6. Attribuer les permission sur le dossier /var/wwww
```shell
cd var
```
```shell
sudo chown -R www-data:www-data ./www
```
### 7. Créer un projet
```shell
cd www
```
```shell
sudo mkdir easygarden
```
```shell
cd easygarden
```
```shell
sudo nano index.html
```
### 8. Activer et configurer le projet sur le serveur Apache
```shell
cd /etc/apache2/sites-available
```
```shell
sudo cp 000-default.conf easygarden.conf
```
```shell
sudo nano easygarden.conf
```
```
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	ServerName easygarden.com
	ServerAlias www.easygarden.com

	DocumentRoot /var/www/easygarden

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```
### 9. Ajouter le projet dans les sites activés (activation)
```shell
sudo a2ensite easygarden.conf
```
### 10. Vérifier si la configuration est correcte
```shell
sudo apache2ctl configtest
```
```shell
service apache2 reload
```
### 11. Editer le fichier C:\Windows\System32\drivers\etc\hosts
```shell
hostname -I
```
### 12. Ajouter la ligne avec l'ip de Ubuntu
```
# Easygarden Ubuntu
172.18.244.37 easygarden.com
```
## CONFIGURATION GENERALE DU SERVEUR APACHE
Fichier de configuration principale d'Apache
```shell
cd etc/apache2/apache2.conf
```

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
