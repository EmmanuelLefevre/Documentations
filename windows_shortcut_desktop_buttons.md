# WINDOWS SHORTCUT DESKTOP BUTTONS
## INTRODUCTION
Tutoriel pour créer des raccourcis sur le bureau Windows afin d'accéder plus rapidement à certaines fonctionnalités qui ne peuvent pas être ajoutées au menu démarrer.
## Basic
⚠️ Reproduire cette manipulation pour chaque raccourci ⚠️

1. Right click sur le bureau > "Nouveau" > "Raccourci"
2. Dans la fenêtre qui s'ouvre, entrez la commande correspondante
3. Bouton "Suivant"
4. Donner un nom au raccourci
5. Bouton "Terminer"
## 🌍 Variables d'environnement 🌍
```shell
rundll32.exe sysdm.cpl,EditEnvironmentVariables
```
Right click sur le raccourci > Propriétés > Avancé > Cocher "Exécuter en tant qu'administrateur"
## 🚀 Services de démarrage 🚀
```shell
services.msc
```
## 🔑 Gestionnaire d'informations d'identification 🔑
Permet de gérer les informations d'identification pour les connexions à des sites web, des réseaux, des applications ou des services Windows spécifiques.
```shell
control /name Microsoft.CredentialManager
```
### 🎨 Personnalisation des icônes 🎨
Right click sur le raccourci > Propriétés > Changer d'icône

Paths des icônes système:
```shell
C:\Windows\System32\shell32.dll
```
```shell
C:\Windows\System32\imageres.dll
```
```shell
C:\Windows\explorer.exe
```

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on my repository (click on the "Star" button at the top right of the repository page). Thanks 🤗
