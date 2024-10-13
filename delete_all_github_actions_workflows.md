# DELETE ALL WORFLOWS FOR A REPOSITORY BY cURL script
## INTRODUCTION
Script cURL permettant de supprimer en masse les exécutions GitHub Actions via l'API GitHub.
## CONFIGURATION
### 1. Créer un token d'accès personnel
### 2. Lui donner les permissions sur les repository et les GitHub Actions
### 3. Script cURL utilisant l'API GitHub
```shell
#!/bin/bash

REPO="EmmanuelLefevre/nom-du-repo" # Remplacer par son dépôt
TOKEN="ton_personal_access_token" # Remplacer par le token crée

# Liste des runs à supprimer
runs=$(curl -s -H "Authorization: token $TOKEN" "https://api.github.com/repos/$REPO/actions/runs?per_page=100" | jq -r '.workflow_runs[].id')

# Boucle pour supprimer chaque run
for run_id in $runs; do
    echo "Deleting workflow run $run_id"
    curl -X DELETE -H "Authorization: token $TOKEN" "https://api.github.com/repos/$REPO/actions/runs/$run_id"
done
```
### 4. Rendre le script exécutable
```shell
chmod +x delete_workflows.sh
```
### 5. Exécuter le script
```shell
./delete_workflows.sh

```