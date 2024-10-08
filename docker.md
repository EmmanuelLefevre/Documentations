# DOCKER
## INTRODUCTION
Plateforme permettant de lancer des applications dans des conteneurs logiciels.
## IMAGE
### Afficher les images disponibles
Lancer le container d'une image
```shell
docker images
```
### Télécharger image NodeJs sans lancer de container
| Command          | Description |
| :--------------: | :---------: |
|`docker pull node`|Télécharger image Node.js par défaut|
|`docker pull node:16`|Télécharger version 16 de Node.js|
|`docker pull node:latest`|Télécharger dernière version de Node.js|
## Lancer container d'une image
```shell
docker run -it node:16
```
## Afficher containers
| Command + option | Description |
| :--------------: | :---------: |
|`docker ps`|Afficher les containers démarrés|
|`docker ps -a`|Afficher tous les containers|
## Stopper container
### 1. Afficher les containers pour récupérer "CONTAINER ID"
```shell
docker ps
```
### 2. Stopper un container spécifique
```shell
docker stop "CONTAINER ID"
```
## FAIRE TOURNER CONTAINER EN ARRIERE PLAN
```shell
docker run -it -d node
```
## NETTOYER SYSTEME
```shell
docker system prune -a
```
## IMAGE LOCALE
### 1. Construire l'image avec un nom et la dernère version
```shell
docker build -t node-test:latest .
```
### 2. Lancer l'image
```shell
docker run -it node-test:latest bash
```
### 3. Construire l'image avec un nom et une version spécifique
```shell
docker build -t node-test:1.0.0 .
```
### 4. Lancer l'image
```shell
docker run -it node-test:1.0.0 bash
```
### 5. Arrêter une image
```shell
docker run -it -v "./app:/app//" node-test bash
```
```shell
docker run -it -v "./pwa:/pwa" node-test bash
```
```shell
docker run -it -v "./app:/app" ubuntu bash
```
## COMMANDES UTILES
- Version Linux du container
```shell
uname -a
```
- Docker compose
```shell
docker-compose up
```
