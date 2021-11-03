# Vérifier que git est installé:
`git -v`
\
`git version`

# Vérifier si un utilisateur est configuré:
`git config --list`
\
`git config user.name`
\
`git config user.email`

# Configurer ou changer d'utilisateur:
`git config --global user.name "user"`
\
`git config --global user.email "user@email.com"`

# Initialiser repository:
`git init`

# Reconfigurer remote origin:
`git remote -v`
\
`git remote rm origin`&nbsp;&nbsp;&nbsp;&nbsp;  <**Effacer l'ancien remote**>
\
`git remote add origin https://github.com/user.name/nomRepo.git`

# Commit:
`git add .`
\
`git commit -m "commentaire"`
\
`git commit -m "commentaire" -a`&nbsp;&nbsp;&nbsp;&nbsp;  <**add + commmit on one line**>
\
`git log --oneline`&nbsp;&nbsp;&nbsp;&nbsp;  <**Afficher la liste des commits**>
\
`git log --date=local`&nbsp;&nbsp;&nbsp;&nbsp;  <**Afficher la liste des commits avec option de commande plus lisible**>
\
`git checkout commitIdentifiant`&nbsp;&nbsp;&nbsp;&nbsp;  <**Se replacer sur un commit**>
\
`git reset --hard`&nbsp;&nbsp;&nbsp;&nbsp;  <**Annuler fichiers ajoutés à `git add .`**>
\
`git commit --amend -m "newMessage"`&nbsp;&nbsp;&nbsp;&nbsp;  <**Modifier le message du dernier commit**>

# Vérifier le statut des fichiers git:
`git status`

# Création d'une branche:
`git branch`&nbsp;&nbsp;&nbsp;&nbsp;  <**Indique la branche sur laquelle on se trouve**>
\
`git branch nameOfBranch`&nbsp;&nbsp;&nbsp;&nbsp;  <**Créer une branche**>
\
`git checkout nameOfBranch`&nbsp;&nbsp;&nbsp;&nbsp;  <**Se déplacer de branche**>
\
`git checkout -b nameOfBranch`&nbsp;&nbsp;&nbsp;&nbsp;  <**Créer une branche et switcher dessus**>

# Fetch
`git fetch --all`&nbsp;&nbsp;&nbsp;&nbsp;  <**git fetch + git merge = git pull**>

# Fusionner (merge):
`git checkout master`&nbsp;&nbsp;&nbsp;&nbsp;  <**Se placer sur la branche master en premier lieu**>
\
`git merge nameOfBranch`&nbsp;&nbsp;&nbsp;&nbsp;  <**Fusionner la branche sur le master**>
\
`git branch -d nameOfBranch`&nbsp;&nbsp;&nbsp;&nbsp;  <**!!!!!!Puis SUPPRIMER la branche**>

# Push:
`git remote add origin https://github.com/user.name/nomRepo.git`&nbsp;&nbsp;&nbsp;&nbsp;  <**Lier local et repo**>
\
`git push origin master`&nbsp;&nbsp;&nbsp;&nbsp;  <**--force => Succeptible de supprimer l'historique des commits**>
               
# Clone/Pull:
`git clone https://github.com/user.name/nomRepo.git`
\
`git pull origin master`

