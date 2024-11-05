# GIT

## INTRODUCTION
Git est un logiciel de gestion de versions décentralisé.

## COMMANDES UTILES
### Version du logiciel
| Command + option | Description |
| :--------------: | :---------: |
|`git -v` ou `git version`|Version de Git|

### Initialiser dépôt local
| Command + option | Description |
| :--------------: | :---------: |
|`git init`|Initialiser nouveau dépôt local Git dans le répertoire actuel (fichier '.git')|

### Utilisateur
| Command + option | Description |
| :--------------: | :---------: |
|`git config --global user.name "user"`|Configurer le nom d'utilisateur global|
|`git config --global user.email "user@email.com"`|Configurer l'adresse e-mail globale|
|`git config --list`| Afficher toutes les configurations actuelles|
|`git config user.name`| Afficher le nom d'utilisateur configuré|
|`git config user.email`| Afficher l'adresse e-mail configurée|

### Clone/Pull:
| Command + option | Description |
| :--------------: | :---------: |
|`git clone https://github.com/user.name/nomRepo.git`|Cloner un dépôt distant vers son local|
|`git pull origin master`|Récupérer les MAJ de la branche 'master' du distant pour les fusionner avec la branche locale actuelle.|

### Synchronisation du distant sur le local
#### 1. Récupérer les dernières modifications du dépôt distant sans les fusionner avec la branche locale
```shell
git fetch origin
```
#### 2. Puis fusionner les modifications de la 'master' distante dans la branche locale actuelle
```shell
git merge origin/master
```

### Reconfigurer dépôt distant
| Command + option | Description |
| :--------------: | :---------: |
|`git remote -v`| Afficher remotes URL (SSH ou HTTPS)|
|`git remote rm origin`|Effacer l'ancien remote nommé "origin"|
|`git remote add origin https://github.com/user.name/nomRepo.git`|Lier le dépôt local au dépôt distant spécifié|

### Commit
| Command + option | Description |
| :--------------: | :---------: |
|`git add .`|Ajouter tous les fichiers modifiés au staging|
|`git commit -m "commentaire"`|Créer un commit avec un message de commentaire|
|`git commit -m "commentaire" -a`|Ajouter et créer un commit en une seule ligne|
|`git checkout commitIdentifiant`|Se replacer sur un commit spécifique|
|`git reset --hard`|Annuler les fichiers ajoutés avec `git add .`|
|`git commit --amend -m "newMessage"`|Modifier le message du dernier commit|
|`git checkout -b nameOfBranch <commitID>`|Récupérer une branche supprimée en pointant vers un commit spécifique|

### Historique
| Command + option | Description |
| :--------------: | :---------: |
|`git log --oneline`|Afficher liste des commits de manière concise|
|`git log --date=local`|Afficher liste des commits avec des dates locales|
|`git log --graph --oneline --decorate`|Afficher liste des commits sous forme graphique avec des références|

### Statut
| Command + option | Description |
| :--------------: | :---------: |
|`git status`|Vérifier le statut des fichiers|

### Branche
| Command + option | Description |
| :--------------: | :---------: |
|`git branch -a`|Afficher toutes les branches|
|`git branch`|Indiquer la branche sur laquelle on se trouve|
|`git branch 'nameOfBranch'`|Créer branche|
|`git checkout 'nameOfBranch'`|Se déplacer vers la branche spécifiée|
|`git switch 'nameOfBranch'`|Se déplacer vers la branche spécifiée|
|`git checkout -b 'nameOfBranch'`|Créer une branche et switcher dessus|
|`git branch -d 'nameOfBranch'`|Supprimer une branche fusionnée|
|`git branch -D 'nameOfBranch'`|Supprimer une branche non fusionnée|

### Récupérer branches distantes
| Command + option | Description |
| :--------------: | :---------: |
|`git fetch --all`|Récupérer toutes les branches/objets du dépôt distant sans les fusionner dans les branches locales (permet d'examiner les MAJ avant de fusionner)|

### Fusionner
#### 1. Se placer sur la branche master
```shell
git checkout master
```
#### 2. Fusionner la branche sur la master
```shell
git merge nameOfBranch
```
#### 3. Puis SUPPRIMER la branche
```shell
git branch -d nameOfBranch
```

### Push
| Command + option | Description |
| :--------------: | :---------: |
|`git remote add origin https://github.com/user.name/nomRepo.git`|Lier le dépôt local au dépôt distant spécifié|
|`git push origin master`|Pousser les commits locaux de la branche 'master' sur la branche 'master' du distant|
|`git push origin master --force`|Succeptible de supprimer l'historique des commits!!!|

### Rebase
| Command + option | Description |
| :--------------: | :---------: |
|`git rebase nameOfBranch`|Déplacer ou combiner une série de commits vers une nouvelle base|

### Stash
| Command + option | Description |
| :--------------: | :---------: |
|`git stash`|Mettre de côté les modifications en cours|

### Diff
| Command + option | Description |
| :--------------: | :---------: |
|`git diff`|Modifications entre branche locale et dernier commit|
|`git diff foo.txt`|Modificationsentre branche locale et dernier commit pour 'foo.txt'|

### Suivi (.gitignore)
| Command + option | Description |
| :--------------: | :---------: |
|`git add -f foo.txt`|Forcer l’ajout de 'foo.txt' (si dans .gitignore)|

### Conflits
| Command + option | Description |
| :--------------: | :---------: |
|`git mergetool`|Utiliser outil de fusion pour résoudre les conflits après une tentative de fusion (permet d'afficher les différences entre les versions conflictuelles)|

### Tag
| Command + option | Description |
| :--------------: | :---------: |
|`git tag -a v1.0 -m "Version 1.0"`|Crée tag annoté pour marquer une version dans l'historique des commits (facilite le suivi des versions)|

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
