# ANGULAR

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [CLI installation](#cli-installation)
- [CLI](#cli)
  - [Nouveau projet](#nouveau-projet)
  - [Serveur](#serveur)
  - [Raccourcis](#raccourcis)
- [COMPODOC URL](#compodoc-url)

## INTRODUCTION
Angular est un framework de développement d'applications web.

## CLI installation
Global:
```shell
npm install -g @angular/cli@16.1.1
```
Projet:
```shell
npm install @angular/cli@16.1.1 --save-dev
```
## CLI
### Nouveau projet
```shell
ng new projectName
```

### Serveur
- Lancer serveur
```shell
ng serve -o
```

- Lancer serveur sur un autre port que le 4000 par défaut
```shell
ng serve -o --port 4001
```

### Raccourcis
| Command | Description |
| :---: | :---: |
| `ng new projectName` | Créer un nouveau projet Angular |
| `ng serve -o` | Lancer le serveur et ouvrir dans le navigateur |
| `ng serve -o --port 4001` | Lancer le serveur sur le port 4001 |
| `ng generate c nomComposant` | Créer un nouveau composant |
| `ng generate c nomComposant --routing` | Créer un nouveau composant avec le routing |
| `ng generate m nomModule` | Créer un nouveau module |
| `ng generate s nomService` | Créer un nouveau service |
| `ng generate guard nomGuard` | Créer un nouveau guard |
| `ng generate interceptor nomInterceptor` | Créer un nouvel interceptor |
| `ng generate class nomClass` | Créer une nouvelle classe |
| `ng generate interface nomInterface` | Créer une nouvelle interface |
| `ng generate library nomLibrary` | Créer une nouvelle bibliothèque |
| `ng generate module nomModule` | Créer un nouveau module |
| `ng generate pipe nomPipe` | Créer un nouveau pipe |
| `ng generate resolver nomResolver` | Créer un nouveau resolver |
| `ng generate service-worker nomService-worker` | Créer un nouveau service-worker |
| `ng generate directive nomDirective` | Créer une nouvelle directive |
| `ng generate app-shell nomAppShell` | Créer un nouvel App Shell |
| `compodoc` | Générer la documentation du projet Angular |

## COMPODOC URL
[Compodoc](http://localhost:8080)

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
