# DWLD IMAGE  
1. Télécharger image NodeJs  
```shell
docker pull node`  
```shell
docker pull node:16`  
```shell
docker pull node:latest`
2. Afficher les images disponibles  
```shell
docker images`
3. Récupérer une image sans lancer de container  
```shell
docker pull node:16`

# LANCER CONTAINER
1. Lancer le container d'une image  
```shell
docker run -it node:16`

# AFFICHER CONTAINER
1. Afficher les containers démarrés  
```shell
docker ps`
2. Afficher tous les containers  
```shell
docker ps -a`  

# STOPPER CONTAINER
1. Afficher les containers afin de récupérer le "CONTAINER ID"  
```shell
docker ps
```
2. Stopper le container désiré  
```shell
docker stop "CONTAINER ID"
```

# FAIRE TOURNER CONTAINER EN ARRIERE PLAN
```shell
docker run -it -d node
```

# NETTOYER SYSTEME
```shell
docker system prune -a
```

# IMAGE LOCALE
1. Construire l'image avec un nom et la version latest
```shell
docker build -t node-test:latest .
```
2. Lancer l'image
```shell
docker run -it node-test:latest bash
```

3. Construire l'image avec un nom et une version spécifique
```shell
docker build -t node-test:1.0.0 .
```
4. Lancer l'image
```shell
docker run -it node-test:1.0.0 bash
```
5. Arrêter une image


```shell
docker run -it -v "./app:/app//" node-test bash
```
```shell
docker run -it -v "./pwa:/pwa" node-test bash
```
```shell
docker run -it -v "./app:/app" ubuntu bash
```

# VERSION LINUX DU CONTAINER  
```shell
uname -a
```

# DOCKER COMPOSE
```shell
docker-compose up
```
