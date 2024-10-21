# POWERSHELL GIT PULL SCRIPT
1. Right click sur le bureau > "Nouveau" > "Raccourci"
2. Dans la fenêtre qui s'ouvre saisir =>
```shell
"C:\Program Files\WindowsApps\Microsoft.PowerShell_7.4.5.0_x64__8wekyb3d8bbwe\pwsh.exe" -ExecutionPolicy Bypass -File "C:\Users\darka\Documents\PowerShell\run_powershell_git_pull_script.ps1"
```
3. Bouton "Suivant"
4. Donner un nom au raccourci
5. Bouton "Terminer"
6. Créer fichier "run_powershell_git_pull_script.ps1"

```shell
# Load PowerShell Profile
. "$HOME\Documents\PowerShell\Microsoft.PowerShell_profile.ps1"

# Call function
custom_function

# Keep window open
Read-Host -Prompt "Press Enter to close"
```
