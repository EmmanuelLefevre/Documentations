# SSH
## INTRODUCTION
Configurer une connexion SSH avec github.com
## CONFIGURATION
### 1. Créer clés
- ED
```shell
ssh-keygen -t ed25519 -C "waiasiaa@protonmail.com" -f ~/.ssh/github_id_ed
```
- RSA
```shell
ssh-keygen -t rsa -b 4096 -C "waiasiaa@protonmail.com" -f ~/.ssh/github_id_ed
```
### 2. Vérifier création clés
```shell
ls ~/.ssh
```
### 3. Démarrer ssh-agent (environnement où les clés sont stockées)
```shell
eval "$(ssh-agent -s)"
```
### 4. Ajouter clé privée à ssh-agent
```shell
ssh-add C:/Users/darka/.ssh/github_id_ed
```
### 5. => github.com Ajouter la clé (Profile>Settings>SSH and GPG keys)
### 6. Open connection
```shell
ssh -T git@github.com
```
### 7. Close connection
```shell
kill <PID>
```
ou
```shell
exit
```
## COMMANDES UTILES
- Lister les clés ajoutées
```shell
ssh add -l
```
- Supprimer toutes les clés ajoutées
```shell
ssh-add -D
```
- Supprimer une clé
```shell
ssh-add -d C:/Users/darka/.ssh/github_id_ed
```
- Agent PID
```shell
eval $(ssh-agent -s)
```
