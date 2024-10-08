# GIT
## INTRODUCTION
Git est un logiciel de gestion de versions décentralisé.
## COMMANDES UTILES
### Version
```shell
git -v
```
```shell
git version
```
### Initialiser dépôt local
```shell
git init
```
### Utilisateur
| Command + option | Description |
| :--------------: | :---------: |
|`git config --global user.name "user"`|Configurer le nom d'utilisateur global pour Git|
|`git config --global user.email "user@email.com"`|Configurer l'adresse e-mail globale pour Git|
|`git config --list`| Afficher toutes les configurations Git actuelles|
|`git config user.name`| Afficher le nom d'utilisateur configuré pour Git|
|`git config user.email`| Afficher l'adresse e-mail configurée pour Git|
### Reconfigurer dépôt distant
| Command + option | Description |
| :--------------: | :---------: |
|`git remote -v`| Afficher les remotes configurés avec leurs URL|
|`git remote rm origin`|Effacer l'ancien remote nommé "origin"|
|`git remote add origin https://github.com/user.name/nomRepo.git`|Ajouter un nouveau remote "origin" avec l'URL spécifiée|
### Commit
| Command + option | Description |
| :--------------: | :---------: |
|`git add .`|Ajouter tous les fichiers modifiés au staging|
|`git commit -m "commentaire"`|Créer un commit avec un message de commentaire|
|`git commit -m "commentaire" -a`|Ajouter et créer un commit en une seule ligne|
|`git log --oneline`|Afficher la liste des commits de manière concise|
|`git log --date=local`|Afficher la liste des commits avec des dates locales|
|`git checkout commitIdentifiant`|Se replacer sur un commit spécifique|
|`git reset --hard`|Annuler les fichiers ajoutés avec `git add .`|
|`git commit --amend -m "newMessage"`|Modifier le message du dernier commit|
### Statut
| Command + option | Description |
| :--------------: | :---------: |
|`git status`|Vérifier le statut des fichiers|
### Branche
| Command + option | Description |
| :--------------: | :---------: |
| `git branch`                      | <**Indique la branche sur laquelle on se trouve**> |
| `git branch 'nameOfBranch'`      | <**Créer une branche**>                          |
| `git checkout 'nameOfBranch'`    | <**Se déplacer de branche**>                     |
| `git checkout -b 'nameOfBranch'` | <**Créer une branche et switcher dessus**>      |
### Récupérer
| Command + option | Description |
| :--------------: | :---------: |
|`git fetch --all`|Récupèrer toutes les branches/objets du dépôt distant sans les fusionner dans les branches locales (permet d'examiner les MAJ avant de fusionner)|
### Fusionner
| Command + option | Description |
| :--------------: | :---------: |
|`git checkout 'master'`| <**Se placer sur la branche master en premier lieu**>|
|`git merge 'nameOfBranch'`| <**Fusionner la branche sur le master**|
|`git branch -d 'nameOfBranch'`| <**!!!!!!Puis SUPPRIMER la branche**|

### Push:
| Command + option | Description |
| :--------------: | :---------: |
`git remote add origin https://github.com/user.name/nomRepo.git`&nbsp;&nbsp;&nbsp;&nbsp;  <**Lier local et repo**>
\
`git push origin master`&nbsp;&nbsp;&nbsp;&nbsp;  <**--force => Succeptible de supprimer l'historique des commits**>

### Clone/Pull:
| Command + option | Description |
| :--------------: | :---------: |
`git clone https://github.com/user.name/nomRepo.git`
\
`git pull origin master`
