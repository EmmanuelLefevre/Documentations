# DWLD IMAGE  
1. Télécharger image NodeJs  
`docker pull node`  
`docker pull node:16`  
`docker pull node:latest`
2. Afficher les images disponibles  
`docker images`
3. Récupérer une image sans lancer de container  
`docker pull node:16`

# LANCER CONTAINER
1. Lancer le container d'une image  
`docker run -it node:16`
2. Afficher les containers démarrés  
`docker ps`
3. Afficher tous les containers  
`docker ps -a`

# STOPPER CONTAINER
1. Afficher les containers afin de récupérer le "CONTAINER ID"  
`docker ps`
2. Stopper le container désiré  
`docker stop "CONTAINER ID"`

# FAIRE TOURNER CONTAINER EN ARRIERE PLAN  
`docker run -it -d node`

# NETTOYER SYSTEME
`docker system prune -a`

# IMAGE LOCALE
1. Construire l'image avec un nom et la version latest  
`docker build -t node-test:latest .`
2. Lancer l'image  
`docker run -it node-test:latest bash`

3. Construire l'image avec un nom et une version spécifique  
`docker build -t node-test:1.0.0 .`
4. Lancer l'image  
`docker run -it node-test:1.0.0 bash`
5. Arrêter une image



`docker run -it -v "./app:/app//" node-test bash`  
`docker run -it -v "./pwa:/pwa" node-test bash`
`docker run -it -v "./app:/app" ubuntu bash `

# VERSION LINUX DU CONTAINER  
`uname -a`

# DOCKER COMPOSE
`docker-compose up`  
``