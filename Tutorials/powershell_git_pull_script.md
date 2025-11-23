# POWERSHELL GIT PULL SCRIPT

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [PROCEDURE](#procedure)
- [BONUS](#bonus)

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
Start-Process -FilePath "C:\Program Files\WindowsApps\Microsoft.PowerShell_7.5.4.0_x64__8wekyb3d8bbwe\pwsh.exe" -ArgumentList "-ExecutionPolicy Bypass -File `"C:\Users\darka\Documents\PowerShell\run_powershell_git_pull_script.ps1`"" -NoNewWindow -Wait
```
⚠️ Also pay attention to the version of powershell installed if you use Windows 11 ...

- Windows 10
```shell
wt.exe -p "PowerShell" -d . -- pwsh.exe -ExecutionPolicy Bypass -File "C:\Users\<UserName>\Documents\PowerShell\run_powershell_git_pull_script.ps1"
```
4. "Next" button

5. Give the shortcut the name you like.

6. "Finish" button

7. ❤️ Additionally give the shortcut a nice icon ❤️

💡 On Windows 10, by default the created shortcut will not have the black PowerShell 7 icon but an other ugly one, you can assign the correct one like this.

Right click on shortcut > Properties > Change icon
Icon path:
```shell
C:\Program Files\WindowsApps\Microsoft.PowerShell_7.5.4.0_x64__8wekyb3d8bbwe\pwsh.exe
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
gpull

# Close terminal
Write-Host ""
Read-Host -Prompt "Press Enter to close... "
```
10. Now you must open your "Microsoft.PowerShell_profile.ps1" file with your favorite text editor.

11. Copy/Paste "git_pull" function and his utility function inside.
```powershell
##########---------- Update your local repositories ----------##########
function gpull {
  [CmdletBinding()]
  param (
    # Force repository information reloading
    [switch]$RefreshCache,
    # Optional parameter to update only one repository
    [string]$Name
  )

  ######## GUARD CLAUSE : GIT AVAILABILITY ########
  if (-not (Get-Command git -ErrorAction SilentlyContinue)) {
    # Helper called to center message nicely
    $msg = "⛔ Git for Windows is not installed (or not found in path)... Install it before using this command ! ⛔"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display message
    Write-Host -NoNewline $paddingStr
    Write-Host $msg -ForegroundColor Red

    return
  }

  ######## START GLOBAL TIMER ########
  $globalTimer = Start-OperationTimer

  ######## CACHE MANAGEMENT ########
  # If cache doesn't exist or if a refresh is forced
  if (-not $Global:GPullCache -or $RefreshCache) {
    # Helper called to center message nicely
    $msg = "🔄 Updating repositories informations... 🔄"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display message
    Write-Host -NoNewline $paddingStr
    Write-Host $msg -ForegroundColor Cyan

    ######## DATA RETRIEVAL ########
    # Function is called only once
    $tempReposInfo = Get-RepositoriesInfo

    ######## GUARD CLAUSE : INVALID CONFIGURATION ########
    # Validate result before caching it
    if ($tempReposInfo -eq $null) {
      # Helper called to center message nicely
      $msg = "⛔ Script stopped due to an invalid configuration ! ⛔"
      $paddingStr = Get-CenteredPadding -RawMessage $msg

      # Display message
      Write-Host -NoNewline $paddingStr
      Write-Host $msg -ForegroundColor Red

      # Exit function
      return
    }

    # If everything is valid cache is created
    $Global:GPullCache = @{
      ReposInfo = $tempReposInfo
    }
  }

  ######## DATA RETRIEVAL ########
  # Retrieve repositories information from cache
  $reposInfo  = $Global:GPullCache.ReposInfo

  $reposOrder = $reposInfo.Order
  $repos      = $reposInfo.Paths
  $username   = $reposInfo.Username
  $token      = $reposInfo.Token

  ######## LAUNCH SCRIPT FOR A SINGLE REPO ########
  $reposToProcess = Get-RepoListToProcess -FullList $reposOrder -TargetName $Name
  # If helper returns `$null` (repo not found), we stop everything
  if ($null -eq $reposToProcess) {
    return
  }

  # Flag initialization
  $isFirstRepo = $true

  ######## REPOSITORY ITERATION ########
  # Iterate over each repository in the defined order
  foreach ($repoName in $reposToProcess) {
    ######## START REPOSITORY TIMER ########
    $repoTimer = Start-OperationTimer

    ######## DATA RETRIEVAL ########
    $repoPath = $repos[$repoName]

    ######## UI : SEPARATOR MANAGEMENT ########
    # Display main separator (except first)
    if (-not $isFirstRepo) {
      Show-MainSeparator
    }
    # Mark first loop is finished
    $isFirstRepo = $false

    ######## GUARD CLAUSE : PATH EXISTS ########
    if (-not (Test-LocalRepoExists -Path $repoPath -Name $repoName)) {
      continue
    }

    ######## GUARD CLAUSE : NOT A GIT REPO ########
    if (-not (Test-IsGitRepository -Path $repoPath -Name $repoName)) {
      continue
    }

    # Change current directory to repository path
    Set-Location -Path $repoPath

    ######## GUARD CLAUSE : REMOTE MISMATCH ########
    # Check local remote matches GitHub info
    if (-not (Test-LocalRemoteMatch -UserName $username -RepoName $repoName)) {
      continue
    }

    ######## MAIN PROCESS ########
    # Show repository name being updated
    Write-Host -NoNewline "🚀 "
    Write-Host -NoNewline "$repoName" -ForegroundColor white -BackgroundColor DarkBlue
    Write-Host " is on update process..." -ForegroundColor Green

    try {
      ######## API CHECK & FETCH ########
      # Check for remote repository existence using GitHub API with authentication token
      $repoUrl = "https://api.github.com/repos/$username/$repoName"
      $response = Invoke-RestMethod -Uri $repoUrl -Method Get -Headers @{ Authorization = "Bearer $token" } -ErrorAction Stop

      # Store original branch to return it later (Trim removes invisible blank spaces)
      $originalBranch = (git rev-parse --abbrev-ref HEAD).Trim()

      # Fetch latest remote changes
      git fetch --prune --quiet

      # Display date of last remote commit
      Show-LastCommitDate

      ######## GUARD CLAUSE : FETCH FAILED ########
      if (-not (Test-GitFetchSuccess -ExitCode $LASTEXITCODE)) {
        $repoIsInSafeState = $false

        continue
        # Move next repository
      }

      ######## DATA RETRIEVAL : TRACKED BRANCHES ########
      # Find all local branches that have a remote upstream
      $branchesToUpdate = Get-LocalBranchesWithUpstream

      ######## GUARD CLAUSE : NO UPSTREAM ########
      # If no branch has an upstream defined, nothing to update or clean up
      if (-not $branchesToUpdate) {
        Write-Host "ℹ️ No upstream defined ! Nothing to update or clean up for this repository ! ℹ️" -ForegroundColor DarkYellow
        continue
      }

      ######## DATA PROCESSING : SORTING ########
      # Organize branches : Main -> Dev -> Others by alphabetical
      $sortedBranchesToUpdate = Get-SortedBranches -Branches $branchesToUpdate

      # Track repository state
      $repoIsInSafeState = $true

      # Track if any branch needed a pull
      $anyBranchNeededPull = $false

      # Separator control
      $branchCount = $sortedBranchesToUpdate.Count
      $i = 0

      ######## UPDATE LOOP ########
      # Iterate over each branch found to pull updates from remote
      foreach ($branch in $sortedBranchesToUpdate) {
        # Increment iteration counter
        $i++

        ######## GUARD CLAUSE : CHECKOUT ########
        if (-not (Invoke-SafeCheckout -TargetBranch $branch.Local -OriginalBranch $originalBranch)) {
          $repoIsInSafeState = $false
          break
        }

        ######## GUARD CLAUSE : LOCAL CONFLICTS ########
        if (-not (Test-WorkingTreeClean -BranchName $branch.Local)) {
          continue
        }

        ######## GUARD CLAUSE : UNPUSHED COMMITS ########
        if (Test-UnpushedCommits -BranchName $branch.Local) {
          continue
        }

        ######## GUARD CLAUSE : ALREADY UP-TO-DATE ########
        if (Test-IsUpToDate -LocalBranch $branch.Local `
                            -RemoteBranch $branch.Remote) {

          # If not last branch, display separator
          if ($i -lt $branchCount) {
            Show-Separator -Length 80 -ForegroundColor DarkGray
          }

          continue
        }

        ######## PULL PROCESS ########
        # If we get here, a branch needs a pull
        $anyBranchNeededPull = $true

        # Execute the update strategy and get the result status
        $updateStatus = Invoke-BranchUpdateStrategy -LocalBranch $branch.Local `
                                                    -RemoteBranch $branch.Remote `
                                                    -RepoName $repoName

        ######## GUARD CLAUSE : UPDATE FAILED ########
        # If update failed critically, mark repo as unsafe and stop
        if ($updateStatus -eq 'Failed') {
          $repoIsInSafeState = $false
          break
        }

        # If execution was successful (Success or Skipped) and not last one, display separator
        if ($updateStatus -ne 'Failed' -and $i -lt $branchCount) {
          Show-Separator -Length 80 -ForegroundColor DarkGray
        }
      }

      ######## STATUS REPORT ########
      # If no branch needed pull and there was more than one branch to check
      if (($anyBranchNeededPull -eq $false) -and ($sortedBranchesToUpdate.Count -gt 1)) {
        Show-Separator -Length 80 -ForegroundColor DarkGray

        Write-Host "All branches are being updated 🤙" -ForegroundColor Green
      }

      ######## DETECT NEW BRANCHES ########
      # Calculate which remote branches are missing locally
      $newBranchesToTrack = Get-NewRemoteBranches

      ######## USER PERMISSION TO PULL NEW BRANCHES ########
      Invoke-NewBranchTracking -NewBranches $newBranchesToTrack

      # Track whether user's branch has been deleted
      [bool]$originalBranchWasDeleted = $false

      ######## CLEANUP : ORPHANED BRANCHES ########
      # Ask to clean branches that no longer exist on remote
      if (Invoke-OrphanedCleanup -OriginalBranch $originalBranch) {
        $originalBranchWasDeleted = $true
      }

      ######## CLEANUP : MERGED BRANCHES ########
      # Ask to clean branches that are already merged into main/dev
      if (Invoke-MergedCleanup -OriginalBranch $originalBranch) {
        $originalBranchWasDeleted = $true
      }

      ######## UI : PRE-CALCULATION ########
      $mergeWillDisplayMessage   = Show-MergeAdvice -DryRun
      $restoreWillDisplayMessage = Restore-UserLocation -RepoIsSafe $repoIsInSafeState `
                                              -OriginalBranch $originalBranch `
                                              -OriginalWasDeleted $originalBranchWasDeleted `
                                              -DryRun

      ######## SEPARATOR MANAGEMENT ########
      # If one OR other
      if ($mergeWillDisplayMessage -or $restoreWillDisplayMessage) {
        Show-Separator -Length 80 -ForegroundColor DarkGray
      }

      ######## WORKFLOW INFO ########
      Show-MergeAdvice

      ######## RETURN STRATEGY ########
      Restore-UserLocation -OriginalBranch $originalBranch `
                          -RepoIsSafe $repoIsInSafeState `
                          -OriginalWasDeleted $originalBranchWasDeleted
    }
    catch {
      ######## ERROR CONTEXT ANALYSIS ########
      # Get HTTP response if exists (regardless of the error type)
      $responseError = $null

      # Response property exists on exception, so we take it
      if ($_.Exception.PSObject.Properties.Match('Response').Count) {
        $responseError = $_.Exception.Response
      }

      ######## ERROR HANDLER : HTTP ########
      # If we have a server response
      if ($null -ne $responseError) {
        Show-GitHubHttpError -StatusCode $responseError.StatusCode `
                            -RepoName $repoName `
                            -ErrorMessage $_.Exception.Message
      }

      ######## ERROR HANDLER : NETWORK / SYSTEM ########
      # No HTTP response
      else {
        Show-NetworkOrSystemError -RepoName $repoName `
                                  -Message $_.Exception.Message
      }
    }

    ######## STOP REPOSITORY TIMER & DISPLAY ########
    Stop-OperationTimer -Watch $repoTimer -RepoName $repoName

    ######## RETURN HOME DIRECTORY ########
    Set-Location -Path $HOME
  }

  ######## STOP GLOBAL TIMER & DISPLAY ########
  if ($reposToProcess.Count -gt 1) {
    Stop-OperationTimer -Watch $globalTimer -IsGlobal
  }
}


#------------------------------#
# GIT PULL UTILITIES FUNCTIONS #
#------------------------------#
##########---------- Get local repositories information ----------##########
function Get-RepositoriesInfo {
  ######## DATA DEFINITION ########
  # GitHub username
  $gitHubUsername = $env:GITHUB_USERNAME

  # GitHub token
  $gitHubToken = $env:GITHUB_TOKEN

  # Array to define the order of repositories
  $reposOrder = @(
    "ArtiWave",
    "Cours",
    "DailyPush",
    "DataScrub",
    "Documentations",
    "Dotfiles",
    "EmmanuelLefevre",
    "GitHubProfileIcons",
    "GoogleSheets",
    "IAmEmmanuelLefevre",
    "MarkdownImg",
    "OpenScraper",
    "ParquetFlow",
    "ReplicaMySQL",
    "Schemas",
    "ScrapMate",
    "Soutenances"
  )

  # Dictionary containing local repositories path
  $repos = @{
    "ArtiWave"               = "$env:USERPROFILE\Desktop\Projets\ArtiWave"
    "Cours"                  = "$env:USERPROFILE\Desktop\Cours"
    "DailyPush"              = "$env:USERPROFILE\Desktop\DailyPush"
    "DataScrub"              = "$env:USERPROFILE\Desktop\Projets\DataScrub"
    "Documentations"         = "$env:USERPROFILE\Documents\Documentations"
    "Dotfiles"               = "$env:USERPROFILE\Desktop\Dotfiles"
    "EmmanuelLefevre"        = "$env:USERPROFILE\Desktop\Projets\EmmanuelLefevre"
    "GitHubProfileIcons"     = "$env:USERPROFILE\Pictures\GitHubProfileIcons"
    "GoogleSheets"           = "$env:USERPROFILE\Desktop\GoogleSheets"
    "IAmEmmanuelLefevre"     = "$env:USERPROFILE\Desktop\Projets\IAmEmmanuelLefevre"
    "MarkdownImg"            = "$env:USERPROFILE\Desktop\MarkdownImg"
    "OpenScraper"            = "$env:USERPROFILE\Desktop\Projets\OpenScraper"
    "ParquetFlow"            = "$env:USERPROFILE\Desktop\Projets\ParquetFlow"
    "ReplicaMySQL"           = "$env:USERPROFILE\Desktop\Projets\ReplicaMySQL"
    "Schemas"                = "$env:USERPROFILE\Desktop\Schemas"
    "ScrapMate"              = "$env:USERPROFILE\Desktop\Projets\ScrapMate"
    "Soutenances"            = "$env:USERPROFILE\Desktop\Soutenances"
  }

  # Error message templates
  $envVarMessageTemplate = "Check {0} in Windows Environment Variables..."
  $functionNameMessage = "in Get-RepositoriesInfo !"

  ######## GUARD CLAUSE : MISSING USERNAME ########
  if ([string]::IsNullOrWhiteSpace($gitHubUsername)) {
    # Helper called to center error message nicely
    $errMsg = "❌ GitHub username is missing or invalid ! ❌"
    $paddingErrStr = Get-CenteredPadding -RawMessage $errMsg

    # Display error message
    Write-Host -NoNewline $paddingErrStr
    Write-Host $errMsg -ForegroundColor Red

    # Helper called to center info message nicely
    $infoMsg = "ℹ️ " + ($envVarMessageTemplate -f "'GITHUB_USERNAME'")
    $paddingInfoStr = Get-CenteredPadding -RawMessage $infoMsg

    # Display info message
    Write-Host -NoNewline $paddingInfoStr
    Write-Host $infoMsg -ForegroundColor DarkYellow

    return $null
  }

  ######## GUARD CLAUSE : MISSING TOKEN ########
  if ([string]::IsNullOrWhiteSpace($gitHubToken)) {
    # Helper called to center error message nicely
    $errMsg = "❌ GitHub token is missing or invalid ! ❌"
    $paddingErrStr = Get-CenteredPadding -RawMessage $errMsg

    # Display error message
    Write-Host -NoNewline $paddingErrStr
    Write-Host $errMsg -ForegroundColor Red

    # Helper called to center info message nicely
    $infoMsg = "ℹ️ " + ($envVarMessageTemplate -f "'GITHUB_TOKEN'")
    $paddingInfoStr = Get-CenteredPadding -RawMessage $infoMsg

    # Display info message
    Write-Host -NoNewline $paddingInfoStr
    Write-Host $infoMsg -ForegroundColor DarkYellow

    return $null
  }

  ######## GUARD CLAUSE : EMPTY ORDER LIST ########
  if (-not $reposOrder -or $reposOrder.Count -eq 0) {
    # Helper called to center error message nicely
    $errMsg = "❌ Local array repo order is empty ! ❌"
    $paddingErrStr = Get-CenteredPadding -RawMessage $errMsg

    # Display error message
    Write-Host -NoNewline $paddingErrStr
    Write-Host $errMsg -ForegroundColor Red

    # Helper called to center info message nicely
    $infoMsg = "ℹ️ Define at least one repository $functionNameMessage (order array)"
    $paddingInfoStr = Get-CenteredPadding -RawMessage $infoMsg

    # Display info message
    Write-Host -NoNewline $paddingInfoStr
    Write-Host $infoMsg -ForegroundColor DarkYellow

    return $null
  }

  ######## GUARD CLAUSE : EMPTY PATH DICTIONARY ########
  if (-not $repos -or $repos.Keys.Count -eq 0) {
    # Helper called to center error message nicely
    $errMsg = "❌ Local repository dictionary is empty ! ❌"
    $paddingErrStr = Get-CenteredPadding -RawMessage $errMsg

    # Display error message
    Write-Host -NoNewline $paddingErrStr
    Write-Host $errMsg -ForegroundColor Red

    # Helper called to center info message nicely
    $infoMsg = "ℹ️ Ensure repository dictionary has valid paths $functionNameMessage"
    $paddingInfoStr = Get-CenteredPadding -RawMessage $infoMsg

    # Display info message
    Write-Host -NoNewline $paddingInfoStr
    Write-Host $infoMsg -ForegroundColor DarkYellow

    return $null
  }

  ######## RETURN SUCCESS ########
  # Helper called to center message nicely
  $msg = "✔️ GitHub and projects configuration are nicely set ✔️"
  $paddingStr = Get-CenteredPadding -RawMessage $msg

  # Display message
  Write-Host -NoNewline $paddingStr
  Write-Host $msg -ForegroundColor Green
  Show-Separator -Length 80 -ForegroundColor DarkBlue
  Write-Host ""

  return @{
    Username = $gitHubUsername
    Token = $gitHubToken
    Order = $reposOrder
    Paths = $repos
  }
}

##########---------- Filter repository list (All vs Single) ----------##########
function Get-RepoListToProcess {
  param (
    [array]$FullList,
    [string]$TargetName
  )

  # No optional parameters, return full list
  if ([string]::IsNullOrWhiteSpace($TargetName)) {
    return $FullList
  }

  # Name specified, check if it exists (case-insensitive)
  if ($FullList -contains $TargetName) {
    # Helper called to center message nicely
    $msg = "🔎 Pull targeted on single repository 🔎"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display message
    Write-Host -NoNewline $paddingStr
    Write-Host "🔎 Pull targeted on single repository 🔎" -ForegroundColor Cyan

    Show-Separator -Length 80 -ForegroundColor DarkGray

    return @($TargetName)
  }

  # Name not found
  else {
    # Helper called to center message nicely
    $msg = "⚠️ Repository `"$($TargetName)`" not found in your configuration list ! ⚠️"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display message
    Write-Host -NoNewline $paddingStr
    Write-Host -NoNewline "⚠️ Repository " -ForegroundColor Red
    Write-Host -NoNewline "`"$($TargetName)`"" -ForegroundColor Magenta
    Write-Host " not found in your configuration list ! ⚠️" -ForegroundColor Red

    return $null
  }
}

##########---------- Check if local repository path exists ----------##########
function Test-LocalRepoExists {
  param (
    [string]$Name,
    [string]$Path
  )

  ######## GUARD CLAUSE : PATH NOT FOUND ########
  # Check if the path variable is defined AND if the folder exists on disk
  if (-not ($Path -and (Test-Path -Path $Path))) {
    # Helper called to center message nicely
    $msg = "⚠️ Local repository path for $Name  doesn't exist ⚠️"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display message
    Write-Host -NoNewline $paddingStr
    Write-Host -NoNewline "⚠️ Local repository path for " -ForegroundColor Red
    Write-Host -NoNewline "$Name" -ForegroundColor White -BackgroundColor Magenta
    Write-Host " doesn't exist ⚠️" -ForegroundColor Red
    Write-Host ""

    Write-Host "Path searched 👉 " -ForegroundColor DarkYellow
    Write-Host "$Path" -ForegroundColor DarkCyan

    return $false
  }

  ######## RETURN SUCCESS ########
  return $true
}

##########---------- Check if folder is a valid git repository ----------##########
function Test-IsGitRepository {
  param (
    [string]$Name,
    [string]$Path
  )

  ######## GUARD CLAUSE : MISSING .GIT FOLDER ########
  # Check if the .git hidden folder exists inside the target path
  if (-not (Test-Path -Path "$Path\.git")) {
    # Helper called to center message nicely
    $msg = "⛔ Local folder $Name found but it's NOT a git repository ⛔"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display message
    Write-Host -NoNewline $paddingStr
    Write-Host -NoNewline "⛔ Local folder " -ForegroundColor Red
    Write-Host -NoNewline "$Name" -ForegroundColor White -BackgroundColor DarkBlue
    Write-Host " found but it's NOT a git repository ⛔" -ForegroundColor Red
    Write-Host ""

    Write-Host "Missing .git folder inside 👉 " -ForegroundColor DarkYellow
    Write-Host "$Path" -ForegroundColor DarkCyan

    return $false
  }

  ######## RETURN SUCCESS ########
  return $true
}

##########---------- Check if local remote matches expected GitHub URL ----------##########
function Test-LocalRemoteMatch {
  param (
    [string]$RepoName,
    [string]$UserName
  )

  ######## DATA RETRIEVAL ########
  # Retrieve the fetch URL for 'origin'
  $rawUrl = (git remote get-url origin 2>$null)

  ######## GUARD CLAUSE : NO REMOTE CONFIGURED ########
  if ([string]::IsNullOrWhiteSpace($rawUrl)) {
    # Helper called to center error message nicely
    $errMsg = "⚠️ No remote 'origin' found for this repository ⚠️"
    $paddingErrStr = Get-CenteredPadding -RawMessage $errMsg

    # Display error message
    Write-Host -NoNewline $paddingErrStr
    Write-Host $errMsg -ForegroundColor Red

    # Helper called to center info message nicely
    $infoMsg = "ℹ️ Repository ignored !"
    $paddingInfoStr = Get-CenteredPadding -RawMessage $infoMsg

    # Helper called to center info message nicely
    Write-Host -NoNewline $paddingInfoStr
    Write-Host $infoMsg -ForegroundColor DarkYellow

    return $false
  }

  ######## GUARD CLAUSE : URL MISMATCH ########
  # Clean up URL because we know it's not empty
  $localRemoteUrl = $rawUrl.Trim()

  # Check if the local remote URL corresponds to the expected GitHub project
  if (-not ($localRemoteUrl -match "$UserName/$RepoName")) {
    # Helper called to center error message nicely
    $errMsg = "⚠️ Original local remote $localRemoteUrl doesn't match ($UserName/$RepoName) ⚠️"
    $paddingErrStr = Get-CenteredPadding -RawMessage $errMsg

    # Display error message
    Write-Host -NoNewline $paddingErrStr
    Write-Host -NoNewline "⚠️ Original local remote " -ForegroundColor Red
    Write-Host -NoNewline "$localRemoteUrl" -ForegroundColor Magenta
    Write-Host -NoNewline " doesn't match (" -ForegroundColor Red
    Write-Host -NoNewline "$UserName" -ForegroundColor Magenta
    Write-Host -NoNewline "/" -ForegroundColor Red
    Write-Host -NoNewline "$RepoName" -ForegroundColor Magenta
    Write-Host ") ⚠️" -ForegroundColor Red

    # Helper called to center info message nicely
    $InfoMsg = "ℹ️ Repository ignored !"
    $paddingInfoStr = Get-CenteredPadding -RawMessage $InfoMsg

    # Display info message
    Write-Host -NoNewline $paddingInfoStr
    Write-Host $InfoMsg -ForegroundColor DarkYellow

    return $false
  }

  ######## RETURN SUCCESS ########
  return $true
}

##########---------- Show last commit date regardless of branch ----------##########
function Show-LastCommitDate {
  ######## DATA RETRIEVAL ########
  # Retrieve all remote branches sorted by date
  $allRefs = git for-each-ref --sort=-committerdate refs/remotes --format="%(refname:short) %(committerdate:iso-strict)" 2>$null

  ######## GUARD CLAUSE : NO REFS RETRIEVED ########
  # Check if we got a result
  if ([string]::IsNullOrWhiteSpace($allRefs)) {
    return
  }

  ######## FILTERING ########
  # Exclude "HEAD" references (ex: origin/HEAD) and select most recent
  $lastCommitInfo = $allRefs | Where-Object {
    ($_ -notmatch '/HEAD\s') -and ($_ -match '/')
  } | Select-Object -First 1

  ######## GUARD CLAUSE : NO MATCHING BRANCH ########
  if ([string]::IsNullOrWhiteSpace($lastCommitInfo)) {
    return
  }

  # Separate the chain into two
  $parts = $lastCommitInfo -split ' ', 2

  ######## GUARD CLAUSE : DATA INTEGRITY FAIL ########
  # Check data integrity (must have Branch + Date)
  if ($parts.Length -ne 2) {
    return
  }

  ######## PROCESSING / DISPLAY ########
  # Clean up branch name (Remove "origin/")
  $branchName = $parts[0] -replace '^.*?/', ''
  $dateString = $parts[1]

  try {
    # Convert ISO string into [datetime] object
    [datetime]$commitDate = $dateString

    # Define culture on "en-US"
    $culture = [System.Globalization.CultureInfo]'en-US'

    # Format date (ex: Monday 13 September 2025)
    $formattedDate = $commitDate.ToString('dddd dd MMMM yyyy', $culture)

    # Display formatted message
    Write-Host -NoNewline "📈 Last repository commit : " -ForegroundColor DarkYellow
    Write-Host -NoNewline "$formattedDate" -ForegroundColor Cyan
    Write-Host -NoNewline " on " -ForegroundColor DarkYellow
    Write-Host "$branchName" -ForegroundColor Magenta

    Show-Separator -Length 80 -ForegroundColor DarkGray
  }
  catch {
    # If date parsing fails, exit silently
    return
  }
}

##########---------- Check if git fetch succeeded ----------##########
function Test-GitFetchSuccess {
  param ([int]$ExitCode)

  ######## GUARD CLAUSE : FETCH FAILED ########
  # Check if the exit code indicates an error (non-zero)
  if ($ExitCode -ne 0) {
    Write-Host -NoNewline "⚠️ "
    Write-Host -NoNewline "'Git fetch' failed ! Check your Git access credentials (SSH keys/Credential Manager)... ⚠️" -ForegroundColor Red

    return $false
  }

  ######## RETURN SUCCESS ########
  return $true
}

##########---------- Retrieve local branches that have a remote upstream ----------##########
function Get-LocalBranchesWithUpstream {
  ######## DATA RETRIEVAL ########
  # Get raw data : LocalBranchName + RemoteUpstreamName
  $rawRefs = git for-each-ref --format="%(refname:short) %(upstream:short)" refs/heads 2>$null

  ######## GUARD CLAUSE : NO REFS ########
  if ([string]::IsNullOrWhiteSpace($rawRefs)) {
    return $null
  }

  ######## DATA PROCESSING ########
  # Convert raw text to objects
  $branchesWithUpstream = $rawRefs | ForEach-Object {
    $parts = $_ -split ' '

    # Check data integrity (Must have: Name + Upstream)
    if ($parts.Length -eq 2 -and -not [string]::IsNullOrWhiteSpace($parts[1])) {
      [PSCustomObject]@{
        Local  = $parts[0]
        Remote = $parts[1]
      }
    }
  }

  return $branchesWithUpstream
}

##########---------- Sort branches by priority (Main > Dev > Others) ----------##########
function Get-SortedBranches {
  param (
    [Parameter(Mandatory=$true)]
    [array]$Branches
  )

  ######## CONFIGURATION ########
  # Defines priority branches names
  $mainBranchNames = @("main", "master")
  $devBranchNames  = @("dev", "develop")

  ######## SORTING LOGIC ########
  # Create three lists to guarantee order (force an array)
  $mainList   = @($Branches | Where-Object { $mainBranchNames -icontains $_.Local })
  $devList    = @($Branches | Where-Object { $devBranchNames -icontains $_.Local })

  # Combines two priority lists for exclusion filter
  $allPriorityNames = $mainBranchNames + $devBranchNames

  # Sort other branches alphabetically
  $otherList  = $Branches | Where-Object { -not ($allPriorityNames -icontains $_.Local) } | Sort-Object Local

  ######## MERGE & RETURN ########
  # Combine lists in the desired order
  return $mainList + $devList + $otherList
}

##########---------- Try to checkout branch, handle errors ----------##########
function Invoke-SafeCheckout {
  param (
    [string]$TargetBranch,
    [string]$OriginalBranch
  )

  ######## CHECKOUT ACTION ########
  git checkout $TargetBranch *> $null

  ######## GUARD CLAUSE : CHECKOUT FAILED ########
  if ($LASTEXITCODE -ne 0) {
    Write-Host "⚠️ "
    Write-Host -NoNewline "Could not checkout " -ForegroundColor Magenta
    Write-Host -NoNewline "$TargetBranch" -ForegroundColor Red
    Write-Host " !!! ⚠️" -ForegroundColor Magenta

    Write-Host -NoNewline "ℹ️ Blocked by local changes on " -ForegroundColor DarYellow
    Write-Host -NoNewline "$OriginalBranch" -ForegroundColor Magenta
    Write-Host ". Halting updates for this repository !" -ForegroundColor DarYellow

    return $false
  }

  ######## SUCCESS FEEDBACK ########
  Write-Host -NoNewline "Inspecting branch " -ForegroundColor Cyan
  Write-Host "$TargetBranch" -ForegroundColor Magenta

  return $true
}

##########---------- Check for local modifications (Staged/Unstaged) ----------##########
function Test-WorkingTreeClean {
  param (
    [string]$BranchName
  )

  ######## DATA RETRIEVAL ########
  $unstagedChanges = git diff --name-only --quiet
  $stagedChanges   = git diff --cached --name-only --quiet

  ######## GUARD CLAUSE : DIRTY TREE ########
  if ($unstagedChanges -or $stagedChanges) {
    Write-Host -NoNewline "󰨈  Conflict detected on " -ForegroundColor Red
    Write-Host -NoNewline "$BranchName" -ForegroundColor Magenta
    Write-Host -NoNewline " , this branch has local changes. Pull avoided... 󰨈" -ForegroundColor Red
    Write-Host "Affected files =>" -ForegroundColor DarkCyan

    # Display details logic
    if ($unstagedChanges) {
      Write-Host "Unstaged affected files =>" -ForegroundColor DarkCyan
      git diff --name-only | ForEach-Object {
        Write-Host "   $_" -ForegroundColor DarkCyan
      }
    }
    if ($stagedChanges) {
      Write-Host "Staged affected files =>" -ForegroundColor DarkCyan
      git diff --cached --name-only | ForEach-Object {
        Write-Host "   $_" -ForegroundColor DarkCyan
      }
    }

    return $false
  }

  ######## RETURN SUCCESS ########
  return $true
}

##########---------- Check if local branch has unpushed commits ----------##########
function Test-UnpushedCommits {
  param (
    [string]$BranchName
  )

  ######## DATA RETRIEVAL ########
  $unpushed = git log "@{u}..HEAD" --oneline -q 2>$null

  ######## GUARD CLAUSE : BRANCH AHEAD ########
  if ($unpushed) {
    Write-Host -NoNewline "⚠️ Branch ahead => " -ForegroundColor Red
    Write-Host -NoNewline "$BranchName" -ForegroundColor Magenta
    Write-Host " has unpushed commits. Pull avoided to prevent a merge ! ⚠️" -ForegroundColor Red

    return $true
  }

  ######## RETURN SUCCESS ########
  return $false
}

##########---------- Check if local branch is already up to date ----------##########
function Test-IsUpToDate {
  param (
    [string]$LocalBranch,
    [string]$RemoteBranch
  )

  ######## DATA RETRIEVAL ########
  $localCommit  = git rev-parse $LocalBranch
  $remoteCommit = (git rev-parse $RemoteBranch -q 2>$null)

  ######## GUARD CLAUSE : : REMOTE MISSING ########
  # Consider "Up to date" to avoid pull errors, or handle separately
  if (-not $remoteCommit) {
    return $true
  }

  ######## GUARD CLAUSE : ALREADY SYNCED ########
  if ($localCommit -eq $remoteCommit) {
    Write-Host -NoNewline "$LocalBranch" -ForegroundColor Red
    Write-Host " is already updated ✅" -ForegroundColor Green

    return $true
  }

  ######## RETURN FAILURE ########
  # Hashes are different, so it's not up to date
  return $false
}

##########---------- Execute pull strategy (Auto vs Interactive) ----------##########
function Invoke-BranchUpdateStrategy {
  param (
    [string]$LocalBranch,
    [string]$RemoteBranch,
    [string]$RepoName
  )

  # Default state
  $pullStatus = 'Skipped'

  ######## STRATEGY : AUTO-UPDATE (Main/Master) ########
  if ($LocalBranch -eq "main" -or $LocalBranch -eq "master") {
    Write-Host "⏳ Updating main branch..." -ForegroundColor Magenta
    Show-LatestCommitMessage -LocalBranch $LocalBranch -RemoteBranch $RemoteBranch -HideHashes

    git pull

    # Check if pull worked
    if ($LASTEXITCODE -eq 0) {
      $pullStatus = 'Success'
    }
    else {
      $pullStatus = 'Failed'
    }
  }

  ######## STRATEGY : AUTO-UPDATE (Dev/Develop) ########
  elseif ($LocalBranch -eq "dev" -or $LocalBranch -eq "develop") {
    Write-Host "⏳ Updating develop branch..." -ForegroundColor Magenta
    Show-LatestCommitMessage -LocalBranch $LocalBranch -RemoteBranch $RemoteBranch -HideHashes

    git pull

    # Check if pull worked
    if ($LASTEXITCODE -eq 0) {
      $pullStatus = 'Success'
    }
    else {
      $pullStatus = 'Failed'
    }
  }

  ######## STRATEGY : INTERACTIVE ########
  # Ask user for other branches
  else {
    Write-Host -NoNewline "Branch " -ForegroundColor Magenta
    Write-Host -NoNewline "$LocalBranch" -ForegroundColor Red
    Write-Host " has updates." -ForegroundColor Magenta

    Show-LatestCommitMessage -LocalBranch $LocalBranch -RemoteBranch $RemoteBranch -HideHashes

    Write-Host -NoNewline "Pull ? (Y/n): " -ForegroundColor Magenta

    # Helper called for a robust response
    $wantToPull = Wait-ForUserConfirmation

    if ($wantToPull) {
      Write-Host -NoNewline "⏳ Updating " -ForegroundColor Magenta
      Write-Host -NoNewline "$LocalBranch" -ForegroundColor Red
      Write-Host "..." -ForegroundColor Magenta

      git pull

      # Check if pull worked
      if ($LASTEXITCODE -eq 0) {
        $pullStatus = 'Success'
      }
      else {
        $pullStatus = 'Failed'
      }
    }
    else {
      Write-Host -NoNewline "Skipping pull for " -ForegroundColor Magenta
      Write-Host -NoNewline "$LocalBranch" -ForegroundColor Red
      Write-Host "..." -ForegroundColor Magenta

      # Reset pull success
      $pullStatus = 'Skipped'
    }
  }

  ######## RESULT FEEDBACK ########
  switch ($pullStatus) {
    'Success' {
      Write-Host -NoNewline "$LocalBranch" -ForegroundColor Red
      Write-Host " successfully updated ✅" -ForegroundColor Green

    }
    'Failed' {
      Write-Host "⚠️ "
      Write-Host -NoNewline "Error updating " -ForegroundColor Red
      Write-Host -NoNewline "$LocalBranch" -ForegroundColor Magenta
      Write-Host -NoNewline " in " -ForegroundColor Red
      Write-Host -NoNewline "$RepoName" -ForegroundColor white -BackgroundColor DarkBlue
      Write-Host " ⚠️" -ForegroundColor Red
    }
  }

  return $pullStatus
}

##########---------- Get and show latest commit message ----------##########
function Show-LatestCommitMessage {
  param (
    [string]$LocalBranch,
    [string]$RemoteBranch,
    [switch]$HideHashes
  )

  ######## DATA RETRIEVAL ########
  # Get HASH HEAD
  $localHash  = git rev-parse $LocalBranch 2>$null
  $remoteHash = git rev-parse $RemoteBranch 2>$null

  ######## GUARD CLAUSE : INVALID REFERENCES ########
  if (-not $localHash -or -not $remoteHash) {
    Write-Host "⚠️ Unable to read local/remote references ! ⚠️" -ForegroundColor Red

    return
  }

  ######## DATA ANALYSIS : ANCESTRY ########
  $isLocalBehind  = git merge-base --is-ancestor $localHash $remoteHash 2>$null
  $isRemoteBehind = git merge-base --is-ancestor $remoteHash $localHash 2>$null

  # Divergence detection (detect rebase/push --force)
  if (-not $isLocalBehind -and -not $isRemoteBehind) {
    Write-Host "⚠️ History rewritten or divergence detected... A pull can trigger a rebase or a reset ! ⚠️" -ForegroundColor Red
  }

  # Get new commits
  $raw = git log --oneline --no-merges "$LocalBranch..$RemoteBranch" 2>$null

  # Normalisation : string → array
  $newCommits = @()
  if ($raw) {
    if ($raw -is [string]) {
      $newCommits = @($raw)
    }
    else {
      $newCommits = $raw
    }
  }

  ######## GUARD CLAUSE : NO COMMITS VISIBLE ########
  # If no commits
  if ($newCommits.Count -eq 0) {
    if ($isLocalBehind) {
      Write-Host "ℹ️ Fast-forward possible (no visible commits) ℹ️" -ForegroundColor DarkYellow
    }
    else {
      Write-Host "ℹ️ No commit visible, but a pull may be needed... ℹ️" -ForegroundColor DarkYellow
    }
    return
  }

  # Cleanup if HideHashes option is enabled → removes hash in front of message
  if ($HideHashes) {
    $newCommits = $newCommits | ForEach-Object {
      ($_ -replace '^[0-9a-f]+\s+', '')
    }
  }

  ######## GUARD CLAUSE : SINGLE COMMIT ########
  # One commit
  if ($newCommits.Count -eq 1) {
    Write-Host -NoNewline "Commit message : " -ForegroundColor Magenta
    Write-Host "`"$($newCommits[0])`"" -ForegroundColor Cyan
    return
  }

  ######## DISPLAY MULTIPLE COMMITS ########
  # Several commits
  Write-Host "New commits received :" -ForegroundColor Magenta
  foreach ($commit in $newCommits) {
    Write-Host "- `"$commit`"" -ForegroundColor Cyan
  }
}

##########---------- Calculate new remote branches to track ----------##########
function Get-NewRemoteBranches {
  ######## DATA RETRIEVAL ########
  # Get all remote branches (excluding HEAD) and local branches
  $allRemoteRefs = git for-each-ref --format="%(refname:short)" refs/remotes | Where-Object { $_ -notmatch '/HEAD$' }
  $allLocalBranches = git for-each-ref --format="%(refname:short)" refs/heads

  # List to store new branches to track
  $branchesFound = @()

  ######## PROCESSING LOOP ########
  # Iterate through remote branches to find those not tracked locally
  foreach ($remoteRef in $allRemoteRefs) {

    # Extract local name from remote ref (ex: "origin/feature/x" -> "feature/x")
    if ($remoteRef -match '^[^/]+/(.+)$') {
      $localEquivalent = $Matches[1]

      ######## GUARD CLAUSE : IGNORED PREFIXES ########
      # Define prefixes to exclude (Hotfix and Release)
      $prefixesToIgnore = @('hotfix/', 'release/')
      $shouldIgnore = $false

      # Check each prefix to ignore
      foreach ($prefix in $prefixesToIgnore) {
        # Check if branch name begins with a prefix to ignore
        if ($localEquivalent.StartsWith($prefix, [System.StringComparison]::OrdinalIgnoreCase)) {
          $shouldIgnore = $true

          # No point in continuing to search, exit loop
          break
        }
      }

      # If branch matches an ignored prefix, skip to next iteration
      if ($isIgnored) {
        continue
      }

      ######## GUARD CLAUSE : ALREADY TRACKED ########
      # If the branch already exists locally, skip it
      if ($localEquivalent -in $allLocalBranches) {
        continue
      }

      ######## ADD TO SELECTION ########
      # If we are here, strictly valid (Not ignored AND Not already local)
      $branchesFound += $remoteRef
    }
  }

  return $branchesFound
}

##########---------- Interactive proposal to create new local branches OR delete remote ----------##########
function Invoke-NewBranchTracking {
  param (
    [array]$NewBranches
  )

  ######## GUARD CLAUSE : NO NEW BRANCHES ########
  # If the list is empty or null, nothing to do
  if (-not $NewBranches -or $NewBranches.Count -eq 0) {
    return
  }

  ######## CONFIGURATION ########
  # Branches that should NEVER be deleted remotely even if user doesn't track them
  $protectedBranches = @("dev", "develop", "main", "master")

  Show-Separator -Length 80 -ForegroundColor DarkGray

  # Flag initialization
  $isFirstLoop = $true

  ######## USER INTERACTION LOOP ########
  foreach ($newBranchRef in $NewBranches) {
    # Extract local name (ex: origin/feature/x -> feature/x)
    $null = $newBranchRef -match '^[^/]+/(.+)$'
    $localBranchName = $Matches[1]

    ######## UI : SEPARATOR MANAGEMENT ########
    # Display separator (except first)
    if (-not $isFirstLoop) {
      Show-Separator -Length 80 -ForegroundColor DarkGray
    }
    # Mark first loop is finished
    $isFirstLoop = $false

    # Display Branch Found
    Write-Host -NoNewline "❤️ New remote branches found => " -ForegroundColor Blue
    Write-Host "🦄 $localBranchName 🦄" -ForegroundColor Red

    # Get and show latest commit message
    $latestCommitMsg = git log -1 --format="%s" $newBranchRef 2>$null
    if ($latestCommitMsg) {
      Write-Host -NoNewline "Commit message : " -ForegroundColor Magenta
      Write-Host "$latestCommitMsg" -ForegroundColor Cyan
    }

    ######## CHOICE 1 : TRACKING ########
    # Ask user permission
    Write-Host -NoNewline "Pull " -ForegroundColor Magenta
    Write-Host -NoNewline "$localBranchName" -ForegroundColor Red
    Write-Host -NoNewline " ? (Y/n): " -ForegroundColor Magenta

    # Helper called for a robust response
    $wantToTrack = Wait-ForUserConfirmation

    ######## YES WE TRACK ########
    if ($wantToTrack) {
      Write-Host -NoNewline "⏳ Creating local branch " -ForegroundColor Magenta
      Write-Host "$localBranchName" -ForegroundColor Red

      # Create local branch tracking remote branch
      git branch --track --quiet $localBranchName $newBranchRef 2>$null

      # Check if branch creation worked
      if ($LASTEXITCODE -eq 0) {
        Write-Host -NoNewline "$localBranchName" -ForegroundColor Red
        Write-Host " successfully pulled ✅" -ForegroundColor Green
      }
      # If branch creation failed
      else {
        Write-Host -NoNewline "$localBranchName" -ForegroundColor Red
        Write-Host "⚠️ New creation branch has failed ! ⚠️" -ForegroundColor Red
      }
    }

    ######## CHOICE 2 : OTHERWISE WE PROPOSE REMOVAL #######
    else {
      # We NEVER suggest deleting a protected branch
      if ($protectedBranches -contains $localBranchName) {
        continue
      }

      Write-Host -NoNewline "ℹ️ You decided not to track " -ForegroundColor DarkYellow
      Write-Host -NoNewline "$localBranchName" -ForegroundColor Red
      Write-Host ". This branch is forsaken ?" -ForegroundColor DarkYellow

      ######## STEP 1 : REMOTE DELETION ########
      Write-Host -NoNewline "🗑️ Delete remote branch permanently ? (Y/n): " -ForegroundColor Magenta

      # Helper called for a robust response
      $wantToDelete = Wait-ForUserConfirmation

      if ($wantToDelete) {
        ######## STEP 2 : DOUBLE CONFIRMATION ########
        Write-Host -NoNewline (" " * 20)
        Show-Separator -Length 40 -ForegroundColor Red

        # Helper called to center message nicely
        $msg = "💀 ARE YOU SURE ? THIS ACTION IS IRREVERSIBLE ! 💀"
        $paddingStr = Get-CenteredPadding -RawMessage $msg

        # Display message
        Write-Host -NoNewline $paddingStr
        Write-Host "💀 ARE YOU SURE ? THIS ACTION IS IRREVERSIBLE ! 💀" -ForegroundColor Red
        Write-Host "Maybe you are near to delete remote branch of one of your team's member..." -ForegroundColor Red
        Write-Host -NoNewline "Confirm deletion of " -ForegroundColor Magenta
        Write-Host -NoNewline "$localBranchName" -ForegroundColor Red
        Write-Host -NoNewline " ? (Y/n): " -ForegroundColor Magenta

        # Helper called for a robust response
        $isSure = Wait-ForUserConfirmation

        if ($isSure) {
          Write-Host -NoNewline "🔥 Removal of " -ForegroundColor Magenta
          Write-Host -NoNewline "$localBranchName" -ForegroundColor Red
          Write-Host "..." -ForegroundColor Magenta

          git push origin --delete $localBranchName 2>&1 | Out-Null

          # Check if branch deletion worked
          if ($LASTEXITCODE -eq 0) {
            Write-Host -NoNewline "☁️ $localBranchName" -ForegroundColor Magenta
            Write-Host " successfully deleted from server" -ForegroundColor Green
          }
          # If branch deletion failed
          else {
            Write-Host -NoNewline "⚠️ Failed to delete " -ForegroundColor Red
            Write-Host "origin/$localBranchName ⚠️" -ForegroundColor Magenta
          }
        }
        else {
          Write-Host -NoNewline "👍 Remote branch " -ForegroundColor Green
          Write-Host -NoNewline "$localBranchName" -ForegroundColor Magenta
          Write-Host " kept 👍" -ForegroundColor Green
        }
      }
      else {
        Write-Host -NoNewline "👍 Remote branch " -ForegroundColor Green
        Write-Host -NoNewline "$localBranchName" -ForegroundColor Magenta
        Write-Host " kept 👍" -ForegroundColor Green
      }
    }
  }
}

##########---------- Interactive cleanup of orphaned (gone) branches ----------##########
function Invoke-OrphanedCleanup {
  param (
    [string]$OriginalBranch
  )

  ######## DATA RETRIEVAL ########
  # Define protected branches (never delete them)
  $protectedBranches = @("dev", "develop", "main", "master")

  # Get current branch name to ensure we don't try to delete it
  $currentBranch = (git rev-parse --abbrev-ref HEAD)

  # Find branches marked as ': gone]' in git verbose output
  $orphanedBranches = git branch -vv | Select-String -Pattern '\[.*: gone\]' | ForEach-Object {
    $line = $_.Line.Trim()
    if ($line -match '^\*?\s*([\S]+)') {
      $Matches[1]
    }
  }

  # Filter, remove protected branches and current branch from list
  $branchesToClean = $orphanedBranches | Where-Object {
    ($_ -ne $currentBranch) -and (-not ($protectedBranches -icontains $_))
  }

  ######## GUARD CLAUSE : NOTHING TO CLEAN ########
  # Original branch was NOT deleted (or empty list)
  if (-not $branchesToClean -or $branchesToClean.Count -eq 0) {
    return $false
  }

  Show-Separator -Length 80 -ForegroundColor DarkGray

  Write-Host "🧹 Cleaning up orphaned local branches..." -ForegroundColor DarkYellow

  $originalWasDeleted = $false

  ######## INTERACTIVE CLEANUP LOOP ########
  foreach ($orphaned in $branchesToClean) {
    # Helper called for warn about stash on branch
    Show-StashWarning -BranchName $orphaned

    # Ask user
    Write-Host -NoNewline "🗑️ Delete the orphaned local branch " -ForegroundColor Magenta
    Write-Host -NoNewline "$orphaned" -ForegroundColor Red
    Write-Host -NoNewline " ? (Y/n): " -ForegroundColor Magenta

    # Helper called for a robust response
    $wantToDelete = Wait-ForUserConfirmation

    if ($wantToDelete) {
      Write-Host -NoNewline "🔥 Removal of " -ForegroundColor Magenta
      Write-Host -NoNewline "$orphaned" -ForegroundColor Red
      Write-Host " branch..." -ForegroundColor Magenta

      # Attempt secure removal
      git branch -d $orphaned *> $null

      # Check if deletion worked
      if ($LASTEXITCODE -eq 0) {
        Write-Host -NoNewline "$orphaned" -ForegroundColor Red
        Write-Host " successfully deleted ✅" -ForegroundColor Green
        if ($orphaned -eq $OriginalBranch) {
          $originalWasDeleted = $true
        }
      }
      # If deletion failed (probably unmerged changes)
      else {
        Write-Host -NoNewline "⚠️ Branch " -ForegroundColor Red
        Write-Host -NoNewline "$orphaned" -ForegroundColor Magenta
        Write-Host " contains unmerged changes ! ⚠️" -ForegroundColor Red

        Write-Host -NoNewline "Force the deletion of " -ForegroundColor Magenta
        Write-Host -NoNewline "$orphaned" -ForegroundColor Red
        Write-Host -NoNewline " ? (Y/n): " -ForegroundColor Magenta

        # Helper called for a robust response
        $wantToForce = Wait-ForUserConfirmation

        if ($wantToForce) {
          # Forced removal
          git branch -D $orphaned *> $null

          # Check if forced deletion worked
          if ($LASTEXITCODE -eq 0) {
            Write-Host -NoNewline "$orphaned" -ForegroundColor Red
            Write-Host " successfully deleted ✅" -ForegroundColor Green

            # Mark original branch as deleted
            if ($orphaned -eq $OriginalBranch) {
              $originalWasDeleted = $true
            }
          }
          # If forced deletion failed
          else {
            Write-Host -NoNewline "⚠️ Unexpected error. Failure to remove " -ForegroundColor Red
            Write-Host "$orphaned ⚠️" -ForegroundColor Magenta
          }
        }
        # User refuses forced deletion
        else {
          Write-Host -NoNewline "👍 Local branch " -ForegroundColor Green
          Write-Host -NoNewline "$orphaned" -ForegroundColor Magenta
          Write-Host " kept 👍" -ForegroundColor Green
        }
      }
    }
  }

  return $originalWasDeleted
}

##########---------- Interactive cleanup of fully merged branches ----------##########
function Invoke-MergedCleanup {
  param (
    [string]$OriginalBranch
  )

  ######## DATA RETRIEVAL ########
  # Define integration branches to check against
  $integrationBranches = @("main", "master", "develop", "dev")

  # Define protected branches (never delete these)
  $protectedBranches   = @("dev", "develop", "main", "master")

  # Get current branch name to ensure we don't try to delete it
  $currentBranch = (git rev-parse --abbrev-ref HEAD).Trim()

  # Use hash table to collect merged branches (avoids duplicates if merged in both dev and main)
  $allMergedBranches = @{}

  foreach ($intBranch in $integrationBranches) {
    # Check if integration branch exists locally
    if (git branch --list $intBranch) {
      # Get branches merged into this integration branch
      git branch --merged $intBranch | ForEach-Object {
        $branchName = $_ -replace '^\*\s+', ''
        $branchName = $branchName.Trim()

        $allMergedBranches[$branchName] = $true
      }
    }
  }

  ######## FILTERING ########
  # Filter list to keep only branches that can be cleaned (not current and not protected ones)
  $branchesToClean = $allMergedBranches.Keys | Where-Object {
    ($_ -ne $OriginalBranch) -and ($_ -ne $currentBranch) -and (-not ($protectedBranches -icontains $_))
  }

  ######## GUARD CLAUSE : NOTHING TO CLEAN ########
  if (-not $branchesToClean -or $branchesToClean.Count -eq 0) {
    # Original branch was NOT deleted
    return $false
  }

  Show-Separator -Length 80 -ForegroundColor DarkGray

  Write-Host "🧹 Cleaning up branches that have already being merged..." -ForegroundColor DarkYellow

  $originalWasDeleted = $false

  ######## INTERACTIVE CLEANUP LOOP ########
  foreach ($merged in $branchesToClean) {
    # Helper called for warn about stash on branch
    Show-StashWarning -BranchName $merged

    # Ask user
    Write-Host -NoNewline "Branch " -ForegroundColor Magenta
    Write-Host -NoNewline "$merged" -ForegroundColor Red
    Write-Host -NoNewline " is already merged. 🗑️ Delete ? (Y/n): " -ForegroundColor Magenta

    # Helper called for a robust response
    $wantToDelete = Wait-ForUserConfirmation

    if ($wantToDelete) {
      Write-Host -NoNewline "🔥 Removal of " -ForegroundColor Magenta
      Write-Host -NoNewline "$merged" -ForegroundColor Red
      Write-Host " branch..." -ForegroundColor Magenta

      # Secure removal (guaranteed to work because we checked --merged)
      git branch -d $merged *> $null

      # Check if deletion worked
      if ($LASTEXITCODE -eq 0) {
        Write-Host -NoNewline "$merged" -ForegroundColor Red
        Write-Host " successfully deleted ✅" -ForegroundColor Green

        # Check if original branch has been deleted
        if ($merged -eq $OriginalBranch) {
          # Mark original branch as deleted
          $originalWasDeleted = $true
        }
      }
      # If deletion failed
      else {
        Write-Host -NoNewline "⚠️ Unexpected error. Failure to remove " -ForegroundColor Red
        Write-Host "$merged ⚠️" -ForegroundColor Magenta
      }
    }
  }

  return $originalWasDeleted
}

##########---------- Check for unmerged commits in main from dev ----------##########
function Show-MergeAdvice {
  param (
    [switch]$DryRun
  )

  ######## MAIN BRANCH DATA RETRIEVAL ########
  # Identify main branch (main or master)
  $mainBranch = if (git branch --list "main") { "main" }
                elseif (git branch --list "master") { "master" }
                else { $null }

  ######## GUARD CLAUSE : MAIN BRANCH NOT FOUND ########
  if (-not $mainBranch) {
    if ($DryRun) { return $false }
    return
  }

  ######## DEV BRANCH DATA RETRIEVAL ########
  # Identify dev branch (develop or dev)
  $devBranch = if (git branch --list "develop") { "develop" }
              elseif (git branch --list "dev") { "dev" }
              else { $null }

  ######## GUARD CLAUSE : DEV BRANCH NOT FOUND ########
  if (-not $devBranch) {
    if ($DryRun) { return $false }
  }

  ######## LOGIC CHECK ########
  # Check for commits in Dev that are not in Main
  $unmergedCommits = git log "$mainBranch..$devBranch" --oneline -q 2>$null

  if ($DryRun) {
    return [bool]$unmergedCommits
  }

  ######## GUARD CLAUSE : ALREADY MERGED ########
  # If result is empty, everything is merged, so we exit
  if (-not $unmergedCommits) { return }

  ######## SHOW ADVICE ########
  Write-Host -NoNewline "ℹ️ $devBranch" -ForegroundColor Magenta
  Write-Host -NoNewline " has commits that are not in " -ForegroundColor DarkYellow
  Write-Host -NoNewline "$mainBranch" -ForegroundColor Magenta
  Write-Host ". Think about merging ! ℹ️" -ForegroundColor DarkYellow
}

##########---------- Restore user to original branch ----------##########
function Restore-UserLocation {
  param (
    [bool]$RepoIsSafe,
    [string]$OriginalBranch,
    [bool]$OriginalWasDeleted,
    [switch]$DryRun
  )

  ######## GUARD CLAUSE : UNSAFE REPO STATE ########
  # Repository isn't in a safe mode, cannot switch branches safely
  if (-not $RepoIsSafe) {
    if ($DryRun) { return $false }

    Write-Host "⚠️ Repo is in an unstable state. Can't return to the branch where you were ! ⚠️" -ForegroundColor Red
    return
  }

  ######## GUARD CLAUSE : ORIGINAL BRANCH DELETED ########
  # Original branch was removed during cleaning, switch to a fallback branch
  if ($OriginalWasDeleted) {
    if ($DryRun) { return $true }

    Write-Host -NoNewline "⚡ Original branch " -ForegroundColor Magenta
    Write-Host -NoNewline "$OriginalBranch" -ForegroundColor Red
    Write-Host " has been deleted..." -ForegroundColor Magenta

    # Determine fallback branch priority
    $fallbackBranch = if (git branch --list "develop") { "develop" }
                      elseif (git branch --list "dev") { "dev" }
                      elseif (git branch --list "main") { "main" }
                      else { "master" }

    Write-Host -NoNewline "😍 You have been moved to " -ForegroundColor DarkYellow
    Write-Host -NoNewline "$fallbackBranch" -ForegroundColor Magenta
    Write-Host " branch 😍" -ForegroundColor DarkYellow

    git checkout $fallbackBranch *> $null
    return
  }

  ######## DATA RETRIEVAL ########
  # Retrieves branch on which script finished its work
  $currentBranch = git rev-parse --abbrev-ref HEAD

  ######## GUARD CLAUSE : ALREADY ON TARGET ########
  # If we are already on original branch, we do nothing and we say nothing
  if ($currentBranch -eq $OriginalBranch) {
    if ($DryRun) { return $false }

    return
  }

  ######## STANDARD RETURN ########
  if ($DryRun) { return $true }

  # Otherwise, we go there and display it
  git checkout $OriginalBranch *> $null

  Write-Host -NoNewline "👌 Place it back on the branch where you were => " -ForegroundColor Magenta
  Write-Host "$OriginalBranch" -ForegroundColor Red
}

##########---------- Check and warn about stashes on a branch ----------##########
function Show-StashWarning {
  param (
    [string]$BranchName
  )

  # Check if this branch has stash files
  $stashCheck = git stash list | Select-String -Pattern "On ${BranchName}:"

  if ($stashCheck) {
    Write-Host -NoNewline "⚠️ WARNING : There are stashes on branch " -ForegroundColor Red
    Write-Host -NoNewline "$BranchName" -ForegroundColor Magenta
    Write-Host -NoNewline " ⚠️" -ForegroundColor Red

    $stashCheck | ForEach-Object {
      Write-Host "  - $($_.Line.Trim())" -ForegroundColor DarkCyan
    }

    Write-Host "ℹ️ Deleting branch doesn't delete stash but you will lose context of it" -ForegroundColor Magenta
  }
}

##########---------- Display HTTP errors ----------##########
function Show-GitHubHttpError {
  param (
    [Parameter(Mandatory=$true)]
    [int]$StatusCode,

    [Parameter(Mandatory=$true)]
    [string]$RepoName,

    [Parameter(Mandatory=$false)]
    [string]$ErrorMessage
  )

  ######## ERROR DISPATCHING ########
  switch ($StatusCode) {
    # Server Errors (all 500+ code)
    { $_ -ge 500 } {
      Write-Host -NoNewline "🔥 "
      Write-Host -NoNewline "GitHub server error (" -ForegroundColor Red
      Write-Host -NoNewline "$StatusCode" -ForegroundColor Magenta
      Write-Host "). GitHub's fault, not yours ! Try later... 🔥" -ForegroundColor Red
      return
    }

    # Not Found
    404 {
      Write-Host -NoNewline "⚠️ "
      Write-Host -NoNewline "Remote repository " -ForegroundColor Red
      Write-Host -NoNewline "$RepoName" -ForegroundColor white -BackgroundColor DarkBlue
      Write-Host " doesn't exists ⚠️" -ForegroundColor Red
      return
    }

    # Rate Limit
    403 {
      Write-Host "󰊤 GitHub API rate limit exceeded! Try again later or authenticate to increase your rate limit. 󰊤" -ForegroundColor Red
      return
    }

    # Unauthorized
    401 {
      Write-Host "󰊤 Check your personal token defined in your settings 󰊤" -ForegroundColor Red
      return
    }

    # Default/Other HTTP errors
    Default {
      Write-Host "⚠️ HTTP Error $StatusCode : $ErrorMessage" -ForegroundColor Red
    }
  }
}

##########---------- Display Network/System errors ----------##########
function Show-NetworkOrSystemError {
  param (
    [Parameter(Mandatory=$true)]
    [string]$RepoName,

    [Parameter(Mandatory=$true)]
    [string]$Message
  )

  ######## ERROR TYPE : NETWORK ########
  # Network problem (DNS, unplugged cable, firewall, no internet ...)
  if ($Message -match "remote name could not be resolved" -or $Message -match "connect" -or $Message -match "timed out") {
    Write-Host -NoNewline "💀 "
    Write-Host -NoNewline "Network error for " -ForegroundColor Red
    Write-Host -NoNewline "$RepoName" -ForegroundColor White -BackgroundColor DarkBlue
    Write-Host ". Unable to connect to GitHub, maybe check your connection or your firewall ! 💀" -ForegroundColor Red
  }

  ######## ERROR TYPE : INTERNAL/SCRIPT ########
  # Script or Git processing error (fallback)
  else {
    Write-Host -NoNewline "💥 Internal Script/Git processing error 💥" -ForegroundColor Red

    # Display technical message for debugging
    Write-Host "Details 👉 " -ForegroundColor Magenta
    Write-Host -NoNewline "$Message" -ForegroundColor Red
  }
}

##########---------- Wait for valid Yes/No user input ----------##########
function Wait-ForUserConfirmation {
  while ($true) {
    $input = Read-Host

    # Matches: Y, y, Yes, yes, YES, or Empty (Enter key)
    if ($input -match '^(Y|y|yes|Yes|YES|^)$') {
      return $true
    }

    # Matches: n, N, no, No, NO
    elseif ($input -match '^(n|N|no|No|NO)$') {
      return $false
    }

    # If invalid input, loop again
    Write-Host "⚠️ Invalid entry... Please type 'y' or 'n' !" -ForegroundColor DarkYellow
    Write-Host -NoNewline "Try again (Y/n): " -ForegroundColor Magenta
  }
}

##########---------- Display a separator line with custom length and colors ----------##########
function Show-Separator {
  param (
    [Parameter(Mandatory=$true)]
    [int]$Length,

    [Parameter(Mandatory=$true)]
    [System.ConsoleColor]$ForegroundColor,

    [Parameter(Mandatory=$false)]
    [System.ConsoleColor]$BackgroundColor,

    [Parameter(Mandatory=$false)]
    [switch]$NoNewline
  )

  ######## DATA PREPARATION ########
  # Create line string based on requested length
  $line = "─" * $Length

  ######## GUARD CLAUSE : WITH BACKGROUND COLOR ########
  # If a background color is specified, handle it specific way and exit
  if ($PSBoundParameters.ContainsKey('BackgroundColor')) {
    Write-Host -NoNewline:$NoNewline $line -ForegroundColor $ForegroundColor -BackgroundColor $BackgroundColor
    return
  }

  ######## STANDARD DISPLAY ########
  # Otherwise (default behavior), display with foreground color only
  Write-Host -NoNewline:$NoNewline $line -ForegroundColor $ForegroundColor
}

##########---------- Display main separator ----------##########
function Show-MainSeparator {
  # Length configuration
  $totalWidth = 80
  $lineLength = 54

  # Calculation of margins
  $paddingCount = [math]::Max(0, [int](($totalWidth - $lineLength) / 2))
  $paddingStr   = " " * $paddingCount

  # Separator display
  Write-Host ""
  Write-Host -NoNewline $paddingStr -ForegroundColor DarkGray
  Show-Separator -NoNewline -Length $lineLength -ForegroundColor DarkGray -BackgroundColor Gray
  Write-Host $paddingStr -ForegroundColor DarkGray
  Write-Host ""
}

##########---------- Start a stopwatch timer ----------##########
function Start-OperationTimer {
  return [System.Diagnostics.Stopwatch]::StartNew()
}

##########---------- Stop timer and display elapsed time ----------##########
function Stop-OperationTimer {
  param (
    [System.Diagnostics.Stopwatch]$Watch,
    [switch]$IsGlobal,
    [string]$RepoName = ""
  )

  $Watch.Stop()
  $time = $Watch.Elapsed

  ######## TEXT TIME CALCULATION ########
  $timeString = ""
  if ($time.TotalMinutes -ge 1) {
    $timeString = "$($time.ToString("mm'm 'ss's'"))"
  }
  else {
    # If was very quick (<1s), we display in ms
    if (-not $IsGlobal -and $time.TotalSeconds -lt 1) {
      $timeString = "$($time.TotalMilliseconds.ToString("0"))ms"
    }
    else {
      $timeString = "$($time.ToString("ss's'"))"
    }
  }

  ######## GLOBAL TIME ########
  if ($IsGlobal) {
    Show-MainSeparator

    # Helper called to center message nicely
    $msg = "✨ All completed in $timeString ✨"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display global timer
    Write-Host -NoNewline $paddingStr
    Write-Host -NoNewline "✨ All completed in " -ForegroundColor Green
    Write-Host -NoNewline "$timeString" -ForegroundColor Magenta
    Write-Host " ✨" -ForegroundColor Green
  }
  ######## REPOSITORY TIME ########
  else {
    Show-Separator -Length 80 -ForegroundColor DarkGray

    # Helper called to center message nicely
    $msg = "⏱️ $RepoName updated in $timeString ⏱️"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display repository timer
    Write-Host -NoNewline $paddingStr
    Write-Host -NoNewline "⏱️ "
    Write-Host -NoNewline "$repoName" -ForegroundColor white -BackgroundColor DarkBlue
    Write-Host -NoNewline " updated in " -ForegroundColor Green
    Write-Host "$timeString ⏱️" -ForegroundColor Magenta
  }
}

##########---------- Calculate centered padding spaces ----------##########
function Get-CenteredPadding {
  param (
    [int]$TotalWidth = 80,
    [string]$RawMessage
  )

  # Standard length in memory
  $visualLength = $RawMessage.Length

  # Range : U+2300 to U+23FF (Technical) and U+2600 to U+27BF (Symbols)
  $emojiPattern = "[\u2300-\u23FF\u2600-\u27BF]"

  # Count how many there are in message
  $hiddenWidth = ([regex]::Matches($RawMessage, $emojiPattern)).Count

  # Add this hidden width to the total length
  $visualLength += $hiddenWidth

  # (Total Width - Message Length) / 2
  # [math]::Max(0, ...) => prevents crash if message is longer than 80 characters
  $paddingCount = [math]::Max(0, [int](($TotalWidth - $visualLength) / 2))

  return " " * $paddingCount
}
```

⚠️ I you don't use a personal token to request the Github API this script will not work. To set up an identification token on the Github API and environements variables, go to the next "Bonus"" section...

![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/git_pull_script.png)

## 😍 Bonus 😍

Request the github api with a personnal token to increase the rate limit and be able to update a private repository...

1. On github.com:  
Settings > Developer settings > Personal access tokens > Tokens (classic) > Generate new token > Generate new token (classic)

- Input "Note": Your token name...
- Expiration option: "No expiration"
- Tick checkbox: "repo"
- Click on "Generate token"

⚠️ Be careful to copy your token because it will no longer be visible afterwards!

2. On windows:  
Setup your username and token in the environment variables.
![First Step](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/git_pull_script_config_environement_variable_step_1.png)  

![Second Step](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/git_pull_script_config_environement_variable_step_2.png)

Repeat operation for the username...  

⚠️ You might need to restart your computer in order that the environment variables will be correctly applied.

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
