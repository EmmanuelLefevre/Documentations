# WINDOWS SHORTCUT DESKTOP BUTTONS
## INTRODUCTION
Tutoriel pour créer des raccourcis sur le bureau Windows afin d'accéder plus rapidement à certaines fonctionnalités qui ne peuvent pas être ajoutées au menu démarrer.
## Basic
Reproduire cette manipulation pour chaque raccourci!

1. Right click sur le bureau > "Nouveau" > "Raccourci"
2. Dans la fenêtre qui s'ouvre, entrez la commande correspondante
3. Bouton "Suivant"
4. Donnez un nom au raccourci
5. Bouton "Terminer"
## Environment variables
```shell
rundll32.exe sysdm.cpl,EditEnvironmentVariables
```
Right click > Propriétés > Avancé
Cocher "Exécuter en tant qu'administrateur"
## Gestionnaire d'informations d'identification
```shell
control /name Microsoft.CredentialManager
```
## Services
```shell
services.msc
```
## Credential manager
```shell
rundll32.exe keymgr.dll,KRShowKeyMgr
```
### Personnalisation des icônes
Right click > Propriétés > Changer d'icône

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