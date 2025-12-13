# GITHUB TRICKS

## SOMMAIRE

- [INTRO](#intro)
- [FILTERREPO](#filter-repo)
- [NOREPLYGITHUBEMAIL](#no-reply-github-email)
- [FICHIERSDECONFIGURATION](#fichiers-de-configuration)
- [SUPPRIMERHISTORIQUEFICHIERSPECIFIQUE](#supprimer-historique-fichier-specifique)
- [SUPPRIMERUNEMAILPUBLIQUE](#supprimer-un-email-publique)
- [METHODESANSFILTER-REPO(NATIVE GIT)](#methode-sans-filter-repo-native-git)
- [BONUS](#bonus)
  - [PROCEDUREBRANCHEDEFAUTMODERNE](#procedure-branche-defaut-moderne)
  - [DELETEALLCONTRIBUTORS](#delete-all-contributors)

## INTRO

Ce document regroupe les procédures pour :  

- nettoyer l'historique Git  
- supprimer des données sensibles  
- supprimer un emailpublique dans les metadonnées de commits  
- uniformiser les contributeurs.  

⚠️ AVERTISSEMENT CRITIQUE ⚠️  

La réécriture d'historique (via `filter-repo` ou `filter-branch`) est destructrice. Elle change les SHA-1 des commits (metadonnées).  

**Faire toujours une COPIE DE SAUVEGARDE complète du dossier local avant de lancer ces commandes**  

Assurez-vous que personne d'autre ne travaille sur le dépôt pendant l'opération...  

## FILTER REPO

`git-filter-repo` est l'outil recommandé par la communauté Git pour réécrire l'historique. Il est basé sur Python.  

**Prérequis :** Python et PIP installés sur la machine.  

- **Installation**

```powershell
pip install git-filter-repo
```

- **Vérification**

```powershell
git filter-repo --version
```

## NO REPLY GITHUB EMAIL

**Sur Github :**  

1. Settings > Email  

2. Set "Keep my email addresses private" sur "On" et copier le "no reply email".  

![Github No Reply Email Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/gitconfig_private_adress.png)

## FICHIERS DE CONFIGURATION

Ces fichiers sont utilisés par certaines commandes ci-dessous pour mapper les anciennes informations vers les nouvelles.  

- **`mailmap`**  

Utilisé pour corriger les noms et emails des auteurs/committers.  

```text
LefevreEmmanuel <47084975+EmmanuelLefevre@users.noreply.github.com> <ancien.email@example.com>
```

- **`requirements.txt`**  

Utilisé pour remplacer du texte dans le corps des fichiers ou les messages de commit.  

```text
email = ancien.email@example.com ==> email = 47084975+EmmanuelLefevre@users.noreply.github.com
```

## SUPPRIMER HISTORIQUE FICHIER SPECIFIQUE

1. Pour effacer totalement un fichier sensible (clés API, mots de passe) de tout l'historique, comme s'il n'avait jamais existé.  

Se placer dans le path du projet.  

```powershell
git filter-repo --path CONTRIBUTORS.md --invert-paths --force
```

2. Rétablissement du remote  

```powershell
git remote add origin git@github.com:<GITHUB_USERNAME>/<REMOTE_NAME>.git
```

3. Forcer le push 

**Note :** Si le push est rejeté, vérifiez que la branche n'est pas "Protected" dans les settings GitHub.  

```powershell
git push origin --force --all
git push origin --force --tags
```

4. Vérification (ne doit rien renvoyer)  

```powershell
git log --all -- example.txt
```

## SUPPRIMER UN EMAIL PUBLIQUE

1. Utiliser `mailmap` et `replacements.txt` pour nettoyer l'historique. Copier ces deux fichiers à la racine de votre projet.  

💡 Dans le cas de vouloir réaliser cette opération sur plusieurs branches il suffit de récupérer toutes les branches distantes avec la commande suivante, sinon passer directement à l'étape 2.  

Se placer dans le path du projet et lancer cette commande  

- **Multi-branches**

```powershell
git fetch --all
```

2. Lancer ensuite la commande de filtrage  

```powershell
git filter-repo --mailmap mailmap --replace-text replacements.txt --force
```

3. Supprimer `mailmap` et `requirements.tx` de votre dépôt local (pour ne pas les publier par erreur)  

4. Nettoyage des résidus locaux pour gagner de la place disque

```powershell
git reflog expire --expire=now --all
git gc --prune=now
```

5. Rétablissement du remote  

```powershell
git remote add origin git@github.com:<GITHUB_USERNAME>/<REMOTE_NAME>.git
```

6. Forcer le push  

**Note :** Désactivez temporairement la protection de branche sur GitHub si nécessaire.  

- **Mono-branche**

```powershell
git push --force origin main --tags
```

- **Multi-branches**

```powershell
git push origin --force --all
git push origin --force --tags
```

7. Vérifications

Afficher les logs pour visualiser si les modifications ont correctement été appliquées.  

```powershell
git log --all --format='%C(yellow)%h %C(cyan)Auteur: %an <%ae>  %C(magenta)Committer: %cn <%ce> %C(reset)%s'
```

Cette commande ne doit rien retourner étant donné que l'email publique a été supprimé.  

```powershell
git log --all --format='%an %ae %cn %ce' | Select-String "public.email@yahoo.fr"
```

8. Republier la branche si nécessaire...  

## METHODE SANS FILTER-REPO (NATIVE GIT)

Si vous ne pouvez pas utiliser Python/git-filter-repo et que vous êtes sous PowerShell.  

Cette commande est plus lente et dépréciée, mais fonctionne partout !  

⚠️ `git filter-branch` est lent. Cette commande force l'identité sur tous les commits.  

1. Uniformisation de l'auteur et du committer  

```powershell
git filter-branch --env-filter '
CORRECT_NAME="EmmanuelLefevre"
CORRECT_EMAIL="47084975+EmmanuelLefevre@users.noreply.github.com"

export GIT_AUTHOR_NAME="$CORRECT_NAME"
export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
export GIT_COMMITTER_NAME="$CORRECT_NAME"
export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
' --tag-name-filter cat -- --all
```

2. Nettoyage post-traitement, suppression des sauvegardes générées par filter-branch  

```powershell
git for-each-ref --format="%(refname)" refs/original/ | ForEach-Object { git update-ref -d $_ }
```

3. Rétablissement du remote  

```powershell
git remote add origin git@github.com:<GITHUB_USERNAME>/<REMOTE_NAME>.git
```

4. Nettoyage des résidus locaux pour gagner de la place disque  

```powershell
git reflog expire --expire=now --all
git gc --prune=now
```

5. Forcer le push  

```powershell
git push origin --force --all
git push origin --force --tags
```

## BONUS

### PROCEDURE BRANCHE DEFAUT MODERNE

Procédure pour moderniser le nom de la branche par défaut => **Master** vers **Main**  

**Sur Github :**  

1. Settings > Branches  

2. Renommer la branche par défaut (master) en main  

**En local :**

3. Se placer sur master  

4. Renommer la branche locale  

```powershell
git branch -m master main
```

5. Récupérer les infos du remote  

```powershell
git fetch origin
```

6. Lier la branche locale main à la branche distante main  

```powershell
git branch -u origin/main main
```

7. Mettre à jour la référence HEAD du remote  

```powershell
git remote set-head origin -a
```

### DELETE ALL CONTRIBUTORS

1. Cette commande réécrit tout l'historique pour qu'il n'y ait plus qu'un seul auteur et committer.  

```powershell
git filter-repo --commit-callback '
new_name = b"EmmanuelLefevre"
new_email = b"47084975+EmmanuelLefevre@users.noreply.github.com"

commit.author_name = new_name
commit.author_email = new_email
commit.committer_name = new_name
commit.committer_email = new_email
' --force
```

2. Rétablissement du remote  

```powershell
git remote add origin git@github.com:<GITHUB_USERNAME>/<REMOTE_NAME>.git
```

3. Forcer le push 

```powershell
git push origin --force --all
git push origin --force --tags
```

4. Nettoyage final (optionnel mais recommandé)  

Une fois que tout est poussé et vérifié, vous pouvez nettoyer les résidus locaux pour gagner de la place disque.  

```powershell
git reflog expire --expire=now --all
git gc --prune=now
```
