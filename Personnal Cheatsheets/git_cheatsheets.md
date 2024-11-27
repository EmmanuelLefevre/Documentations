# GIT

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [COMMANDES UTILES](#commandes-utiles)
  - [Version du logiciel](#version-du-logiciel)
  - [Initialiser dépôt local](#initialiser-dépôt-local)
  - [Utilisateur](#utilisateur)
  - [Clone/Pull](#clonepull)
  - [Push](#push)
  - [Reconfigurer dépôt distant](#reconfigurer-dépôt-distant)
  - [Commit](#commit)
  - [Branche](#branche)
  - [Reset](#reset)
  - [Historique](#historique)
  - [Statut](#statut)
  - [Récupérer branches distantes](#récupérer-branches-distantes)
  - [Rebase](#rebase)
  - [Stash](#stash)
  - [Diff](#diff)
  - [Suivi (.gitignore)](#suivi-gitignore)
  - [Conflits](#conflits)
  - [Tags](#tags)
  - [Synchronisation du distant sur le local](#synchronisation-du-distant-sur-le-local)
  - [Fusionner](#fusionner)
- [RESSOURCES UTILES](#ressources-utiles)

## INTRODUCTION
Git est un logiciel de gestion de versions décentralisé.

## COMMANDES UTILES
### Version du logiciel
| Command + option | Description |
| :--------------: | :---------: |
| `git -v` ou `git version` | Version de Git. |

### Initialiser dépôt local
| Command + option | Description |
| :--------------: | :---------: |
| `git init` | Initialiser nouveau dépôt local Git dans le répertoire actuel (fichier '.git'). |

### Utilisateur
| Command + option | Description |
| :--------------: | :---------: |
| `git config --global user.name "<Username>"` | Configurer le nom d'utilisateur global. |
| `git config --global user.email "<UserEmail>"` | Configurer l'adresse e-mail globale. |
| `git config --list` | Afficher toutes les configurations actuelles. |
| `git config user.name` | Afficher nom d'utilisateur configuré. |
| `git config user.email` | Afficher adresse e-mail configurée. |

### Clone/Pull
| Command + option | Description |
| :--------------: | :---------: |
| `git clone https://github.com/<Username>/<RepositoryName>.git` | Cloner dépôt distant vers local en HTTPS. |
| `git clone git@github.com:<Username>/<RepositoryName>` | Cloner dépôt distant vers local en SSH. |
| `git pull origin master` | Récupérer MAJ branche 'master' du distant pour les fusionner avec la branche locale actuelle. |

### Push
| Command + option | Description |
| :--------------: | :---------: |
| `git push origin master` | Pousser les commits locaux de la branche 'master' sur la branche 'master' du distant. |
| `git push origin master --force` | Succeptible de supprimer l'historique des commits!!! |

### Reconfigurer dépôt distant
| Command + option | Description |
| :--------------: | :---------: |
| `git remote -v`| Afficher remotes URL (SSH ou HTTPS). |
| `git remote rm origin` | Effacer l'ancien remote nommé "origin". |
| `git remote add origin https://github.com/<Username>/<RepositoryName>.git` | Lier dépôt local au dépôt distant en HTTPS. |
| `git remote add origin git@github.com:<Username>/<RepositoryName>` | Lier dépôt local au dépôt distant en SSH. |

### Commit
| Command + option | Description |
| :--------------: | :---------: |
| `git add .` | Ajouter tous les fichiers modifiés au staging. |
| `git add foo.txt` | Ajouter un fichier modifié au staging. |
| `git commit -m "commentaire"` | Créer un commit avec un message de commentaire. |
| `git commit -m "commentaire" -a` | Ajouter et créer un commit en une seule ligne. |
| `git checkout <commitHash>` | Se replacer sur un commit spécifique. |
| `git commit --amend -m "newMessage"` | Modifier le message du dernier commit. |
| `git checkout -b <nameOfBranch> <commitHash>` | Récupérer une branche supprimée en pointant vers un commit spécifique. |

### Branche
| Command + option | Description |
| :--------------: | :---------: |
| `git branch` | Indiquer la branche sur laquelle on se trouve. |
| `git branch -a` | Afficher toutes les branches. |
| `git branch -r` | Afficher toutes les branches disponibles sur le dépôt distant. |
| `git branch <nameOfBranch>` | Créer branche. |
| `git checkout <nameOfBranch>` | Se déplacer vers la branche spécifiée. |
| `git checkout -b <nameOfBranch>` | Créer branche et se déplacer dessus. |
| `git switch <nameOfBranch>` | Se déplacer vers la branche spécifiée. |
| `git checkout -b <nameOfBranch>` | Créer branche et switcher dessus. |
| `git branch -d <nameOfBranch>` | Supprimer branche locale fusionnée. |
| `git branch -D <nameOfBranch>` | Supprimer branche locale non fusionnée. |
| `git push origin --delete <nameOfBranch>` | Supprimer branche distante. |

### Reset
| Command + option | Description |
| :--------------: | :---------: |
| `git reset --soft <CommitHash>` | Réinitialise le pointeur HEAD à <CommitHash> et garde les changements dans l'index (permet de refaire un commit sans perdre de modifications). |
| `git reset --mixed <CommitHash>` | Réinitialise le pointeur HEAD à <CommitHash> et garde les changements dans le répertoire de travail mais réinitialise l'index. |
| `git reset --hard <CommitHash>` | Réinitialise le pointeur HEAD à <CommitHash>, réinitialise l'index et le répertoire de travail, perdant ainsi toutes les modifications non commises. |
| `git reset --hard` | Réinitialise le pointeur HEAD à la dernière révision, réinitialise l'index et le répertoire de travail, perdant toutes les modifications non commitées. |

| Command | Modifie l'index | Modifie répertoire de travail | Perte de modifications non sauvegardées |
| :--------------: | :---------: | :---------: | :---------: |
| `git reset --soft <CommitHash>` | Oui | Non | Non |
| `git reset --mixed <CommitHash>` | Oui | Non | Non |
| `git reset --hard <CommitHash>` | Oui | Oui | Oui |
| `git reset --hard` | Oui | Oui | Oui |

### Historique
| Command + option | Description |
| :--------------: | :---------: |
| `git log --oneline` | Afficher liste des commits de manière concise. |
| `git log --date=local` | Afficher liste des commits avec des dates locales. |
| `git log --graph --oneline --decorate` | Afficher liste des commits sous forme graphique avec des références. |

### Statut
| Command + option | Description |
| :--------------: | :---------: |
| `git status` | Vérifier le statut des fichiers. |

### Récupérer branches distantes
| Command + option | Description |
| :--------------: | :---------: |
| `git fetch --all` | Récupérer toutes les branches/objets du dépôt distant sans les fusionner dans les branches locales (permet d'examiner les MAJ avant de fusionner). |

### Rebase
| Command + option | Description |
| :--------------: | :---------: |
| `git rebase <nameOfBranch> | Déplacer ou combiner une série de commits vers une nouvelle base. |

### Stash
| Command + option | Description |
| :--------------: | :---------: |
| `git stash` | Mettre en attente modifications. |
| `git stash list` | Afficher historique modifications en attente. |
| `git stash apply` | Récupérer dernier stash. |
| `git stash apply stash@{number}` | Récupérer un stash spécifique. |
| `git stash pop` | Appliquer dernier stash et le supprimer de la liste. |
| `git stash drop "stash@{number}"` | Supprimer stash spécifique sans l'appliquer. |
| `git stash clear` | Supprimer toutes les modifications en attente et l'historique. |
| `git stash branch <nameOfBranch>` | Créer branche contenant les modifications du stash. |

### Diff
| Command + option | Description |
| :--------------: | :---------: |
| `git diff` | Modifications entre branche locale et dernier commit. |
| `git diff foo.txt` | Modificationsentre branche locale et dernier commit pour 'foo.txt'. |

### Suivi (.gitignore)
| Command + option | Description |
| :--------------: | :---------: |
| `git add -f foo.txt` | Forcer l’ajout de 'foo.txt' (si dans .gitignore). |

### Conflits
| Command + option | Description |
| :--------------: | :---------: |
| `git mergetool` | Utiliser outil de fusion pour résoudre les conflits après une tentative de fusion (permet d'afficher les différences entre les versions conflictuelles). |

### Tag
| Command + option | Description |
| :--------------: | :---------: |
| `git tag -a v1.0 -m "Version 1.0"` | Crée tag annoté pour marquer une version dans l'historique des commits (facilite le suivi des versions). |

### Synchronisation du distant sur le local
#### 1. Récupérer les dernières modifications du dépôt distant sans les fusionner avec la branche locale
```shell
git fetch origin
```
#### 2. Puis fusionner les modifications de la 'master' distante dans la branche locale actuelle
```shell
git merge origin/master
```

### Fusionner
#### 1. Se placer sur la branche master
```shell
git checkout master
```
```shell
git switch master
```
#### 2. Synchroniser la branche master distante sur la locale
```shell
git pull origin master
```
#### 3. Fusionner la branche cible sur la master
```shell
git merge <nameOfBranch>
```
#### 4. Supprimer la branche locale
```shell
git branch -d <nameOfBranch>
```
#### 5. Puis la branche distante
```shell
git push origin --delete <nameOfBranch>
```

## RESSOURCES UTILES
[Learn Git Plateform](https://learngitbranching.js.org/?locale=fr_FR)  
[Learn Git Official Book](https://git-scm.com/book/en/v2)

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
