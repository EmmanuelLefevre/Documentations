# DELETE ALL WORFLOWS FOR A REPOSITORY BY cURL script
## INTRODUCTION
Script cURL permettant de supprimer en masse les exécutions GitHub Actions via l'API GitHub.
## CONFIGURATION
### 1. Créer un token d'accès personnel
- Developper settings>Personal access tokens>Tokens (classic)
- Generate new token>Generate new token (classic)
- Nom du token => DeleteWorkflows
- Expiration (ici "No expiration")
- Permissions => Cocher "repo" puis "workflow"
- Générer le token
### 2. Script cURL utilisant l'API GitHub
```shell
REPO="EmmanuelLefevre/nom-du-repo" # Remplacer par son dépôt
TOKEN="ton_personal_access_token" # Remplacer par le token crée

# Lister les runs à supprimer
runs=$(curl -s -H "Authorization: token $TOKEN" "https://api.github.com/repos/$REPO/actions/runs?per_page=100" | jq -r '.workflow_runs[].id')

# Boucler pour supprimer chaque run
for run_id in $runs; do
    echo "Deleting workflow run $run_id"
    curl -X DELETE -H "Authorization: token $TOKEN" "https://api.github.com/repos/$REPO/actions/runs/$run_id"
done
```
### 3. WSL Terminal
#### 1. Rendre le script exécutable
```shell
chmod +x delete_workflows.sh
```
#### 2. Installer la bibliothèque jq
- NOTE: jq est un outil en ligne de commande conçu pour traiter, manipuler et interroger des données au format JSON. Il permet aux utilisateurs d'extraire des valeurs spécifiques, de transformer des structures JSON, et de formater la sortie de manière lisible. jq offre une syntaxe puissante et expressive pour filtrer et transformer des données, ce qui en fait un outil très utile pour les développeurs et les administrateurs système travaillant avec des API ou des fichiers JSON.
```shell
sudo apt-get update
```
```shell
sudo apt-get install jq
```
```shell
sudo apt-get upgrade
```
#### 4. Exécuter le script
Dans le WSL =>
```shell
cd /mnt/c/Users/darka/Desktop/Scripts
```
```shell
./delete_workflows.sh

```