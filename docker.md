# DOCKER
## INTRODUCTION
Plateforme permettant de lancer des applications dans des conteneurs logiciels
## TELECHARGER IMAGE
### 1. Télécharger image NodeJs
```shell
docker pull node
```
```shell
docker pull node:16
```
```shell
docker pull node:latest
```
### 2. Afficher les images disponibles
```shell
docker images
```
### 3. Récupérer une image sans lancer de container
```shell
docker pull node:16
```
## LANCER CONTAINER
- Lancer le container d'une image
```shell
docker run -it node:16
```
## AFFICHER CONTAINER
### 1. Afficher les containers démarrés
```shell
docker ps
```
### 2. Afficher tous les containers
```shell
docker ps -a
```
## STOPPER CONTAINER
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
