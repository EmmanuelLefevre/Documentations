# COMMANDES
1. Afficher les distributions installées  
`wsl -l -v`
2. Définir la distribution utilisée  
`wsl --set-default Ubuntu-22.04`
3. Configurer le WSL2 pour toutes les distributions qui seront installées  
`wsl --set-default-version 2`
4. Stopper la distribution  
`wsl --shutdown`

# MARIADB INSTALL
1. Afficher les services de la distribution  
`sudo service --status-all`
2. Installer Mariadb  
`sudo service mariadb status`
3. Démarrer Mariadb  
`sudo service mariadb start`
4. Sécuriser l'installation  
`sudo mysql_secure_installation`

# CREATE ADMIN USER
1. Se connecter à Mariadb  
`sudo mariadb`
2. Créer user admin  
`GRANT ALL ON *.* TO 'admin'@'localhost' IDENTIFIED BY 'nimda' WITH GRANT OPTION;`
3. Remettre à jour les permissions  
`FLUSH PRIVILEGES;`
4. Quitter Mariadb  
`exit`
5. Checker si l'admin a bien été crée  
`mysqladmin -u admin -p version`

# CREATE USER
1. Se connecter en admin  
`mysql -u admin -p`
2. Afficher les databases  
`SHOW DATABASES;`
3. Créer une bdd  
`CREATE DATABASE easygarden;`
4. Créer un user  
`CREATE USER 'user'@'%' IDENTIFIED BY 'xx';`
5. Afficher tous les utilisateurs  
`SELECT user FROM mysql.user;`  
6. Ajouter des droits au user sur une bdd  
`GRANT ALL PRIVILEGES ON easygarden.* TO 'user'@'%';`
7. Remettre à jour les permissions  
`FLUSH PRIVILEGES;`
8. Afficher liaison user/bdd + droits  
`SHOW GRANTS FOR 'user'@'%';`
9. Se connecter en user  
`mysql -u user -p`

# NETSTAT
1. Installer Netstat  
`sudo apt install net-tools`
2. Afficher les connexions réseaux  
`sudo netstat -aln`

# SE CONNECTER DANS LINUX DEPUIS WINDOWS AVEC HEIDISQL
1. Récupérer son ip  
`ip a | grep eth0`  
=> la 1ère ip des deux sans le slash et ce qui suit.

2. Changer la config de Mariadb pour écouter depuis l'extérieur  
`cd /etc/mysql`  
`sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf`
3. Commenter la ligne bind-adress ou remplacer 127.0.0.1 par l'ip ethernet de windows

# APACHE2 INSTALLATION
1. Installer Apache2  
`sudo apt install apache2`
2. Statut du service apache2  
`sudo service apache2 status`
3. Démarrer service apache2  
`sudo service apache2 start`
4. Afficher le hostname par défaut  
`hostname -I`
5. Afficher liste des utilisateurs Apache  
`sudo cat /etc/passwd`
6. Attribuer permission sur le dossier /wwww  
`cd var`  
`sudo chown -R www-data:www-data ./www`
7. Créer projet  
`cd www`  
`sudo mkdir easygarden`  
`cd easygarden`  
`sudo nano index.html`  
8. Activer et configurer le projet sur le serveur Apache  
`cd /etc`  
`cd apache2/sites-available`  
`sudo cp 000-default.conf easygarden.conf`  
`sudo nano easygarden.conf`  
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
9. Ajouter le projet dans les sites activés  
`sudo a2ensite easygarden.conf`
10. Vérifier si la configuration est correcte  
`sudo apache2ctl configtest`  
`service apache2 reload`
11. Editer le fichier C:\Windows\System32\drivers\etc\hosts et ajouter la ligne avec l'ip de Ubuntu  
`hostname -I`
```
# Easygarden Ubuntu
172.18.244.37 easygarden.com
```

# CONFIGURATION GENERALE DU SERVEUR APACHE
1. Fichier de configuration principale d'Apache  
`cd etc/apache2/apache2.conf`
