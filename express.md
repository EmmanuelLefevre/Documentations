# EXPRESS
## INTRODUCTION
Express est un framework d'applications Web back-end pour la création d'API REST avec Node.js (publié en tant que logiciel libre et open source sous la licence MIT).
## COMMANDES UTILES
### Installation
#### 1. Initialiser un projet Node.js
```shell
npm init -y
```
#### 1. Installer Express
```shell
npm install express
```
### Démarrer serveur
#### Node.js
```shell
node ./bin/www
```
#### Nodemon
```shell
nodemon ./bin/www
```
#### NPM script
#### 1. Fichier package.json
```json
scripts": {
    "start": "node -r dotenv/config api/server.js",
    "dev": "npm-run-all --parallel sass-watch nodemon-watch"
},
```
#### 2. Terminal
```shell
npm run dev
```
### Debug
node --inspect ./bin/www
nodemon --inspect ./bin/www         Avec nodemon
chrome://inspect                    Dev tools chrome debuggeur

