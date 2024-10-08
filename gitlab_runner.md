# GITLAB LOCAL RUNNER
GitLab Runner est une application qui fonctionne avec GitLab pour exécuter des tâches CI/CD dans un pipeline.
## PROCEDURE
### 1. Lancer Powershell en admin
[Doumentation Microsoft](https://docs.microsoft.com/en-us/powershell/scripting/windows-powershell/starting-windows-powershell?view=powershell-7#with-administrative-privileges-run-as-administrator)
### 2. Créer un dossier sur le système dans l'emplacement C:\GitLab-Runner
Ce dossier servira de répertoire de travail pour GitLab Runner
```shell
New-Item -Path 'C:\GitLab-Runner' -ItemType Directory
```
### 3. Se placer dans le répertoire
```shell
cd C:\GitLab-Runner
```
### 4. Télécharger le binaire
Télécharger l'éxécutable de GitLab Runner à partir de l'URL spécifiée et l'enregistrer sous le nom gitlab-runner.exe dans le path C:\GitLab-Runner
```shell
Invoke-WebRequest -Uri "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-windows-amd64.exe" -OutFile "gitlab-runner.exe"
```
### 5. Installer GitLab Runner
```shell
.\gitlab-runner.exe install
```
### 6. Lancer GitLab Runner
```shell
.\gitlab-runner.exe start
```
### 7. Arrêter GitLab Runner
```shell
.\gitlab-runner.exe stop
```
## COMMANDES UTILES
### Lister les runners
```shell
.\gitlab-runner.exe list
```
### Désinstaller le service GitLab Runner
```shell
.\gitlab-runner.exe uninstall
```
### Etat du runner (en cours ou arrêté)
```shell
.\gitlab-runner.exe status
```
### MAJ
```shell
.\gitlab-runner.exe update
```
### Logs
Afficher 1000 dernières lignes des logs de GitLab Runner
```shell
Get-Content -Path "C:\GitLab-Runner\logs\gitlab-runner.log" -Tail 1000
```
### Exécuter un job manuellement
Démarrer le runner en mode d'exécution manuelle
```shell
.\gitlab-runner.exe run
```
### Configuration
Modifier les paramètres de configuration existants pour le runner
```shell
.\gitlab-runner.exe configure
```