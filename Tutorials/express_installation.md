# EXPRESS

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [INSTALLATION](#installation)
  - [Initialiser un projet Node.js](#initialiser-un-projet-nodejs)
  - [Installer Express](#installer-express)
  - [Installer Nodemon en développement](#installer-nodemon-en-développement)
- [CREATION SERVEUR](#creation-serveur)
  - [Démarrer serveur](#démarrer-serveur)
    - [Node.js](#nodejs)
    - [Nodemon](#nodemon)
    - [NPM script](#npm-script)
- [DEBUG](#debug)

## INTRODUCTION
Express est un framework d'applications Web back-end pour la création d'API REST avec Node.js (publié en tant que logiciel libre et open source sous la licence MIT).

## INSTALLATION
### 1. Initialiser un projet Node.js
```shell
npm init
```
### 2. Installer Express
```shell
npm install express
```
### 3. Installer Nodemon en développement
```shell
npm install --save-dev nodemon
```
## CREATION SERVEUR
Fichier app.js
```js
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
    res.send('Hello World!');
});

app.listen(port, () => {
    console.log(`Server is running at http://localhost:${port}`);
});
```
### Démarrer serveur
Se placer dans le répertoire du projet!
#### Node.js
```shell
node app.js
```
#### Nodemon
```shell
nodemon app.js
```
#### NPM script
#### 1. Fichier package.json
```json
{
    "name": "newProject",
    "version": "1.0.0",
    "main": "app.js",
    "scripts": {
        "start": "node ./bin/www",
        "dev": "npm-run-all --parallel nodemon-watch"
    },
}
```
#### 2. Terminal
Prod
```shell
npm start
```
Dev
```shell
npm run dev
```
## DEBUG
### Redémarrer le serveur avec le débogueur intégré de Node.js
```shell
node --inspect ./bin/www
```
### Si utilisation de Nodemon
```shell
nodemon --inspect ./bin/www
```
### Accéder à l'outil de débogage dans Chrome, ouvrir l'URL suivante =>
```shell
chrome://inspect
```

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
