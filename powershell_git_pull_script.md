# POWERSHELL GIT PULL SCRIPT
## INTRODUCTION
This tutorial shows the step-by-step procedure to create a powershell script (executable using a shortcut button on the user's desktop) allowing you to update your local repositories with a single click. Indeed, when you have several computers, it can be laborious to synchronize your local repositories each one after the other if you have made a modification in one of them.  
⚠️ This procedure is intended to automate pulls for repositories with only one branch, such as a repository for documentation or configurations.  
👌 Many controls have been added 👌
## PROCEDURE
1. Get the fully path where PowerShell was installed:
```shell
(Get-Command pwsh).Source
```
2. Right click on desktop > "New" > "Shortcut"

3. In the window that opens, enter this line =>

💡 Consider replacing the installation path of your file "run_powershell_git_pull_script.ps1" with yours, it may be different!  

- Windows 11
```shell
"C:\Program Files\WindowsApps\Microsoft.PowerShell_7.4.5.0_x64__8wekyb3d8bbwe\pwsh.exe" -ExecutionPolicy Bypass -File "C:\Users\darka\Documents\PowerShell\run_powershell_git_pull_script.ps1"
```
⚠️ Also pay attention to the version of powershell installed if you use Windows 11 ...

- Windows 10
```shell
wt.exe -p "PowerShell" -d . -- pwsh.exe -ExecutionPolicy Bypass -File "C:\Users\darka\Documents\PowerShell\run_powershell_git_pull_script.ps1"
```
4. "Next" button

5. Give the shortcut the name you like.

6. "Finish" button

7. ❤️ Additionally give the shortcut a nice icon ❤️

💡 On Windows 10, by default the created shortcut will not have the black PowerShell 7 icon but an other ugly one, you can assign the correct one like this.

Right click on shortcut > Properties > Change icon
Icon path:
```shell
C:\Program Files\WindowsApps\Microsoft.PowerShell_7.4.6.0_x64__8wekyb3d8bbwe\pwsh.exe
```
8. Create the file "run_powershell_git_pull_script.ps1" in this path:
```powershell
New-Item -Path "$env:USERPROFILE\Documents\PowerShell\run_powershell_git_pull_script.ps1" -ItemType File
```
9. Copy/Paste this inside the new file
```powershell
# Load PowerShell Profile
. "$env:USERPROFILE\Documents\PowerShell\Microsoft.PowerShell_profile.ps1"

# Call function
git_pull

# Close terminal
Write-Host ""
Read-Host -Prompt "Press Enter to close... "
```
10. Now you must open your "Microsoft.PowerShell_profile.ps1" file with your favorite text editor.

11. Copy/Paste "git_pull" function and his utility function inside.
```powershell
########## Update your local repositories ##########
function git_pull {
  # Get local repositories information and their order
  $reposInfo = Get-RepositoriesInfo
  $reposOrder = $reposInfo.Order
  $repos = $reposInfo.Paths
  # Get GitHub username
  $username = $reposInfo.Username

  # Iterate over each repository in the defined order
  foreach ($repoName in $reposOrder) {
    $repoPath = $repos[$repoName]
    if (Test-Path -Path $repoPath) {
      # Change current directory to repository path
      Set-Location -Path $repoPath

      # Show the name of the repository being updated
      Write-Host -NoNewline "$repoName" -ForegroundColor Magenta
      Write-Host " is on update process 🚀"

      try {
        # Check for remote repository existence using GitHub API
        $repoUrl = "https://api.github.com/repos/$username/$repoName"
        $response = Invoke-RestMethod -Uri $repoUrl -Method Get -ErrorAction Stop

        # Check current branch
        $currentBranch = git rev-parse --abbrev-ref HEAD
        # If branch isn't "master" or "main"
        if ($currentBranch -ne "main" -and $currentBranch -ne "master") {
          Write-Host -NoNewline "⚠️ "
          Write-Host -NoNewline "$repoName" -ForegroundColor Magenta
          Write-Host -NoNewline " is on " -ForegroundColor Red
          Write-Host -NoNewline "$currentBranch" -ForegroundColor Magenta
          Write-Host " not 'main' or 'master'! Cancelling update ⚠️" -ForegroundColor Red
          Write-Host "--------------------------------------------------------------------"

          # Next repository
          continue
        }

        # Check if local changes exist before pull
        $diffOutput = git diff --name-only
        if ($diffOutput) {
          Write-Host "󰨈  Conflict detected! Pull avoided... 󰨈" -ForegroundColor Red
          Write-Host "Affected files =>"
          foreach ($file in $diffOutput) {
            Write-Host " $file" -ForegroundColor DarkCyan
          }
          Write-Host "--------------------------------------------------------------------"

          # Next repository
          continue
        }

        # Check if repository is already updated
        git fetch
        $localCommit = git rev-parse HEAD
        $remoteCommit = git rev-parse "origin/$currentBranch"

        if ($localCommit -eq $remoteCommit) {
          Write-Host "Already updated 🤙" -ForegroundColor Green
          Write-Host "--------------------------------------------------------------------"

          # Next repository
          continue
        }

        # Execute the git pull command if everything is correct
        git pull

        # Check if the command was successful
        if ($LASTEXITCODE -eq 0) {
          Write-Host "Successfully updated ✅" -ForegroundColor Green
        }
        else {
          Write-Host -NoNewline "⚠️ "
          Write-Host -NoNewline "Error updating " -ForegroundColor Red
          Write-Host -NoNewline "$repoName" -ForegroundColor Magenta
          Write-Host " ⚠️" -ForegroundColor Red
        }
      }
      catch {
        # Check if the error is related to the remote repository not existing
        if ($_.Exception.Response.StatusCode -eq 404) {
          Write-Host -NoNewline "⚠️ "
          Write-Host -NoNewline "Remote repository " -ForegroundColor Red
          Write-Host -NoNewline "$repoName" -ForegroundColor Magenta
          Write-Host " doesn't exists ⚠️" -ForegroundColor Red
        }
        # elseif ($responseBody.message -match "API rate limit exceeded") {
        elseif ($_.Exception.Response.StatusCode -eq 403) {
          Write-Host "󰊤 GitHub API rate limit exceeded! Try again later or authenticate to increase your rate limit. 󰊤" -ForegroundColor Red
        }
        else {
          Write-Host -NoNewline "⚠️ An error occurred while updating "
          Write-Host -NoNewline "$repoName" -ForegroundColor Magenta
          Write-Host ": ${_} ⚠️" -ForegroundColor Red
        }
      }

      # Line separator after each repository processing
      Write-Host "--------------------------------------------------------------------"

      # Return to home directory
      Set-Location -Path $HOME
    }
    else {
      Write-Host -NoNewline "⚠️ Local repository " -ForegroundColor Red
      Write-Host -NoNewline "$repoName" -ForegroundColor Magenta
      Write-Host " doesn't exists ⚠️" -ForegroundColor Red
      Write-Host "--------------------------------------------------------------------"
    }
  }
}

########## Get local repositories information ##########
function Get-RepositoriesInfo {
  # GitHub username
  $gitHubUsername = "<YOUR GITHUB USERNAME>"

  # Array to define the order of repositories
  $reposOrder = @("Documentations", "EmmanuelLefevre", "IAmEmmanuelLefevre", "Schemas", "Settings", "Soutenances")

  # Dictionary containing local repositories path
  $repos = @{
    "Documentations"        = "$env:USERPROFILE\Documents\Documentations"
    "EmmanuelLefevre"       = "$env:USERPROFILE\Desktop\Projets\EmmanuelLefevre"
    "IAmEmmanuelLefevre"    = "$env:USERPROFILE\Desktop\Projets\IAmEmmanuelLefevre"
    "Schemas"               = "$env:USERPROFILE\Desktop\Schemas"
    "Settings"              = "$env:USERPROFILE\Desktop\Settings"
    "Soutenances"           = "$env:USERPROFILE\Desktop\Soutenances"
  }

  return @{
    Username = $gitHubUsername
    Order = $reposOrder
    Paths = $repos
  }
}
```
⚠️ I you don't use a personal token to request the Github API don't forget to switch the visibility of your remote repository to public, if not this script will not be able to update your local repository. To set up an identification token on the Github API, go to the next section...

## 😍 Bonus 😍

Request the github api with a personnal token to increase the rate limit and be able to update a private repository...

On github.com:  
Settings > Developer settings > Personal access tokens > Tokens (classic) > Generate new token > Generate new token (classic)

- Input "Note": Your token name...
- Expiration option: "No expiration"
- Tick checkbox: "repo"
- Click on "Generate token"

⚠️ Be careful to copy your token because it will no longer be visible afterwards!

You must now modify the utility function, replace it by:
```powershell
########## Get local repositories information ##########
function Get-RepositoriesInfo {
  # GitHub username
  $GitHubUsername = "<YOUR GITHUB USERNAME>"

  # GitHub token
  $gitHubToken = "<YOUR PERSONAL TOKEN>"

  # Array to define the order of repositories
  $reposOrder = @("Documentations", "EmmanuelLefevre", "IAmEmmanuelLefevre", "Schemas", "Settings", "Soutenances")

  # Dictionary containing local repositories path
  $repos = @{
    "Documentations"        = "$env:USERPROFILE\Documents\Documentations"
    "EmmanuelLefevre"       = "$env:USERPROFILE\Desktop\Projets\EmmanuelLefevre"
    "IAmEmmanuelLefevre"    = "$env:USERPROFILE\Desktop\Projets\IAmEmmanuelLefevre"
    "Schemas"               = "$env:USERPROFILE\Desktop\Schemas"
    "Settings"              = "$env:USERPROFILE\Desktop\Settings"
    "Soutenances"           = "$env:USERPROFILE\Desktop\Soutenances"
  }

  return @{
    Username = $GitHubUsername
    Token = $gitHubToken
    Order = $reposOrder
    Paths = $repos
  }
}
```
At last add the below line after `$username = $reposInfo.Username`
```powershell
$token = $reposInfo.Token
```
And change the line `$response = Invoke-RestMethod -Uri $repoUrl -Method Get -ErrorAction Stop` by this one =>
```powershell
$response = Invoke-RestMethod -Uri $repoUrl -Method Get -Headers @{ Authorization = "Bearer $token" } -ErrorAction Stop
```

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
