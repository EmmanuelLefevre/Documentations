# GITLAB RUNNER LOCAL
## INTRODUCTION
GitLab Runner est une application qui fonctionne avec GitLab pour exécuter des tâches CI/CD dans un pipeline.
## PROCEDURE
### 1. Lancer Powershell en admin
[Doumentation Microsoft](https://docs.microsoft.com/en-us/powershell/scripting/windows-powershell/starting-windows-powershell?view=powershell-7#with-administrative-privileges-run-as-administrator)
### 2. Crée un dossier sur le système à l'emplacement spécifié (C:\GitLab-Runner)
Ce dossier servira de répertoire de travail pour GitLab Runner.
```shell
New-Item -Path 'C:\GitLab-Runner' -ItemType Directory
```
### 3. Se placer dans le répertoire
```shell
cd C:\GitLab-Runner
```
### 4. Télécharger le binaire
Télécharge le fichier exécutable de GitLab Runner à partir de l'URL spécifiée et l'enregistre sous le nom gitlab-runner.exe dans le répertoire C:\GitLab-Runner.
```shell
Invoke-WebRequest -Uri "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-windows-amd64.exe" -OutFile "gitlab-runner.exe"
```
### 5. Installer le runner
```shell
.\gitlab-runner.exe install
```
### 6. Lancer le runner
```shell
.\gitlab-runner.exe start
```
