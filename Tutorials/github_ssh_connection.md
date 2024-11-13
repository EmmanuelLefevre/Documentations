# SSH

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [CONFIGURATION](#configuration)
  - [1. Créer clés](#1-créer-clés)
    - [WSL](#wsl)
    - [Windows Powershell](#windows-powershell)
  - [2. Vérifier création clés](#2-vérifier-création-clés)
  - [3. Démarrer ssh-agent](#3-démarrer-ssh-agent)
  - [4. Ajouter clé privée à ssh-agent](#4-ajouter-clé-privée-à-ssh-agent)
  - [5. Ajouter la clé](#5-ajouter-la-clé)
  - [6. Tester la connexion](#6-tester-la-connexion)
  - [7. Configurer le repository local en SSH](#7-configurer-le-repository-local-en-ssh)
- [COMMANDES UTILES](#commandes-utiles)

## INTRODUCTION
Configurer une connexion SSH avec github.com

## CONFIGURATION
### 1. Créer clés
#### WSL
- ED
```shell
ssh-keygen -t ed25519 -C "<YOUR_GITHUB_EMAIL>" -f ~/.ssh/github_id_ed
```
- RSA
```shell
ssh-keygen -t rsa -b 4096 -C "<YOUR_GITHUB_EMAIL>" -f ~/.ssh/github_id_rsa
```
- ECDSA
```shell
ssh-keygen -t ecdsa -b 521 -C "<YOUR_GITHUB_EMAIL>" -f ~/.ssh/github_id_ecdsa
```
#### Windows Powershell
- ED
```shell
ssh-keygen -t ed25519 -C "<YOUR_GITHUB_EMAIL>" -f "$env:USERPROFILE\.ssh\github_id_ed"
```
- RSA
```shell
ssh-keygen -t rsa -b 4096 -C "<YOUR_GITHUB_EMAIL>" -f "$env:USERPROFILE\.ssh\github_id_rsa"
```
- ECDSA
```shell
ssh-keygen -t ecdsa -b 521 -C "<YOUR_GITHUB_EMAIL>" -f "$env:USERPROFILE\.ssh\github_id_ecdsa"
```

### 2. Vérifier création clés
```shell
ls ~/.ssh
```
### 3. Démarrer ssh-agent
Le ssh-agent est l'environnement où les clés sont stockées...
```shell
eval "$(ssh-agent -s)"
```
### 4. Ajouter clé privée à ssh-agent
```shell
ssh-add C:/Users/<UserName>/.ssh/github_id_ed
```
### 5. Ajouter la clé
=> github.com  
Profile > Settings > SSH and GPG keys
### 6. Tester la connection
```shell
ssh -T git@github.com
```
### 7. Configurer le repository local en SSH
Récupérer le lien SSH dans le repository distant.  

Se placer dans le path du repository local!
```shell
git remote set-url origin git@github.com:<UserName>/<RepoName>.git
```
Vérifier la configuration:
```shell
git remote -v
```
Le retour de la commande doit affiché ce format =>  
`origin git@github.com:<UserName>/<RepoName>.git`

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
ssh-add -d C:/Users/<UserName>/.ssh/github_id_ed
```
- Agent PID
```shell
eval $(ssh-agent -s)
```

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
