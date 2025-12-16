# POWERSHELL UPDATE GIT REPOSITORIES SCRIPT

## SOMMAIRE

- [INTRO](#intro)
- [WHY THIS SCRIPT](#why-this-script)
- [WORKFLOW](#workflow)
- [REQUIREMENTS](#requirements)
- [INSTALLATION PROCEDURE](#installation-procedure)
- [BONUS](#bonus)
- [CONSOLE APPLICATION SCREENS](#console-application-screens)

## INTRO

🚀 No more struggling to sync Git between my desktop PC and my laptop ! 🚀  

This documentation details the architecture of Update-GitRepositories, a PowerShell automation module acting as a local orchestrator. It is designed to maintain the integrity of your development environment by updating, cleaning, and synchronizing all your local repositories with a single command or automatically at startup.  

## WHY THIS SCRIPT

😫 **The Problems =>**  

1. 🔄 **Sync Anxiety & Multi-Machine Chaos**  
Working on a desktop and a laptop often leads to "Drift". You start coding on your laptop, realizing too late that you forgot to pull the latest changes you made yesterday on your desktop.  

- **Solution,** this script runs automatically at startup. You sit down, and your machine is already up-to-date.
- **Peace of Mind,** eliminates the risk of working on an outdated version or facing avoidable merge conflicts.

2. 🧠 **Mental Load & Repetitive Tasks**  
Manually checking 10, 20, or 30 repositories every morning is a waste of mental energy. A simple batch script often fails because it blindly tries to pull without context.  

- **Solution,** an intelligent Orchestrator. It iterates through your specific list of active projects, skipping the irrelevant ones.
- **Efficiency**, one shortcut, 2 seconds of execution, 100% visibility.

3. 🛡️ **Safety & "Dirty" State Protection**  
Standard scripts break things. If you have uncommitted work (dirty tree) or unpushed commits, a blind git pull can create a mess.  

- **Solution,** the script features strict Guard Clauses. It checks if your workspace is clean before touching anything.
- **Bot Detection :** it automatically detects and resets specific bot branches (like output) to ensure they match the remote exactly.

4. 🆕 **New Machine Setup**  
When switching to a new laptop, I used to spend hours manually cloning my 30+ repositories one by one.  

- **Solution,** just copy this script and the config. Run gpull. It detects missing folders and offers to clone everything for you. Instant setup.

5. 📍 **Context Awareness (The "Smart Restore")**  
Most update scripts force a checkout to main, pulling changes, and leave you there. You lose your spot.  

- **State Preservation,** the script remembers you were on feature/login-page. It switches to main, updates it, cleans up old branches, and puts you back exactly where you were.

6. 🧹 **Hygiene & Garbage Collection**  
Over time, local repositories get cluttered with dead branches (feature/done-3-months-ago) that have already been merged or deleted remotely.  

- **Garbage Collector,** it proactively purposes to delete orphaned branches (: gone) and fully merged branches, keeping your local list clean.

🏗 **ARCHITECTURE**  

Flow Controller based on iterative & sequential, stateless and awareness.  

🧠 **PHILOSOPHY**

- **Safety First (Guard Clause)**
- **UX**
- **Cross-Platform**

⚡ **TRIGGER**

- **Event-Driven** (startup computer)
- **GUI** (desktop shortcut)
- **CLI** (alias Powershell: gpull)

🛠️ **FEATURES**

1. 📦 **Multi-Branch Update**

- **Prioritization,** main and develop branches
- **Targeting,** optional parameter for updating 1 repository
- **Read-Only Mode,** able to restrict updates to main/master only (IsOnlyMain). Ideal for documentation or course repositories where you don't need feature branches.
- **Silent Auto-Update** on integration branches
- **Interactive Mode** on incoming commits from other branches
- **Bot Detection** (sync force)

2. 🧹 **Garbage Collector**

- **Orphaned Cleanup,** detects/removes orphaned branches (interactive)
- **Merged Cleanup,** identifies/removes already merged branches (interactive)
- **Protection,** prevents the deletion of an integration branch

3. 🛡️ **Safety and Integrity (Safety Checks)**

- **Dirty Tree Protection,** pull canceled if files are not committed
- **Unpushed Protection,** pull canceled if local commits are not pushed
- **Stash Warning**

4. 🎛️ **Context Awareness & Restoration**

- **State Preservation,** remembers the active branch
- **Smart Restore,** replaces the user on the original branch
- **Fallback Logic,** if the original branch is deleted, replace the user on the development branch.

5. 🔍 **Divergence Analysis**

- **History Analysis,** calculates the number of commits Ahead/Behind
- **Log Preview,** displays the latest incoming commit messages
- **Divergence Alert,** detects divergent histories

6. 🛰️ **Discovery and Monitoring**

- **New Branch Detection,** scans the remote branch to track new remote branches
- **Tracking Proposal,** creates a local branch (or interactively deletes an obsolete branch)

7. 🏗️ **Auto-Provisioning & Onboarding**
New computer? No problem.  

- **Missing Repo Detection,** if a repository defined in your config doesn't exist locally, the script doesn't fail
- **Remote Validation,** it queries the GitHub API to ensure the repository actually exists (avoiding errors on typos)
- **Interactive Cloning,** it proposes to clone the missing repository via SSH immediately
- **Auto-Structure,** it automatically creates missing parent directories (if the Projets folder is missing, it creates it)

5. 📈 **Visual and Concise Reporting**

- **Real-Time Feedback**
- **Summary Table,** status and precise duration for each repository
- **UI Polish,** dynamic separators and text centering

9. 🔐 **GitHub API Integration and Security**

- **Repository access via GitHub API** + pre-verification of repository existence and access rights
- **Secure Configuration,** Environment Variables (Token/Username)

10. ⏱️ **Performance and Caching**

- **Metrics,** global and individual timers
- **Session Cache,** load configuration once per session

11. 🌐 **Granular and Resilient Error Handling**

- **HTTP context,** API error differentiation
- **Network Resilience,** timeout and DNS management without crashing the orchestrator
- **Isolation** in case of repository failure (Try/Catch pattern)

👌 Others many controls and features have been added 👌

## REQUIREMENTS

PowerShell Core is the cross-platform version of PowerShell, built on .NET Core. It works on Windows, Linux, and macOS. It is distinct from Windows PowerShell (v5.1 and earlier), which is still shipped with Windows but is no longer actively developed with new features.  
For cross-platform scripting and modern features, PowerShell 7 (or higher) is the recommended version.  

⚠️ You NEED to install it ! ⚠️  

## WORKFLOW

1. **Initialization :**  
The Get-DefaultGlobalGitIgnoreTemplate function holds the source of truth (OS files, IDEs, Languages...).

2. **Validation Loop :**  
Iterates through the defined repository list (triggered by Update-GitRepositories or its alias gpull).

- **Existence Check :**  
If missing : Checks GitHub API -> Proposes Clone -> Clones -> Marks as "✨ Cloned"  
If present : proceeds to standard checks
- **Integrity Check :**  
Verifies the folder exists and is a valid Git repository
- **Security Check :**  
Ensures the local origin remote matches the expected GitHub URL

3. **Update Strategy :**

- **Fetch :**  
Performs a git fetch --prune to refresh remote references
- **Prioritize :**  
Sorts branches (Main/Master -> Dev/Develop -> others)
- **Action :**  
Auto-updates integration branches + triggers Interactive Mode for feature branches

4. **Maintenance: :**

- **Safety :**  
Blocks updates if the working tree is dirty or has unpushed commits
- **Cleanup :**  
Proposes deletion for orphaned (gone) or fully merged branches

5. **Reporting :**  
Generates a summary table with execution time and status :  

- ✨ Already Updated
- 🐙 Cloned
- ❌ Failed
- 🙈 Ignored
- ⏩ Skipped
- ✅ Updated

## INSTALLATION PROCEDURE

### Setup the desktop shortkey (Windows only)

1. For Windows 10 get the fully path where PowerShell was installed :

```shell
(Get-Command pwsh).Source
```

2. Right click on desktop > "New" > "Shortcut"

3. In the window that opens, enter this line =>

💡 Consider replacing the installation path of your file "run_powershell_git_pull_script.ps1" with yours, it may be different!  

- Windows 10

```shell
Start-Process -FilePath "C:\Program Files\WindowsApps\Microsoft.PowerShell_7.5.4.0_x64__8wekyb3d8bbwe\pwsh.exe" -ArgumentList "-ExecutionPolicy Bypass -File `"C:\Users\darka\Documents\PowerShell\run_powershell_git_pull_script.ps1`"" -NoNewWindow -Wait
```

⚠️ Also pay attention to the version of powershell installed if you use Windows 10 ...

- Windows 11

```shell
wt.exe -p "PowerShell" pwsh.exe -ExecutionPolicy Bypass -File "C:\Users\<UserName>\Documents\PowerShell\run_powershell_git_pull_script.ps1"
```

4. "Next" button

5. Give the shortcut the name you like.

6. "Finish" button

7. Create the folder (if it doesn't already exist) :  

```powershell
$dir = Split-Path $PROFILE -Parent; if (!(Test-Path $dir)) { New-Item -Path $dir -ItemType Directory }; Set-Location $dir
```

8. Create the file (if it doesn't already exist) :  

```powershell
if (!(Test-Path ".\Microsoft.PowerShell_profile.ps1")) { New-Item ".\Microsoft.PowerShell_profile.ps1" -ItemType File }
```

9. Copy/Paste this inside the new file

```powershell
# Load PowerShell Profile dynamically (works on all OS paths)
. $PROFILE

# Call function
Update-GitRepositories

# Close terminal
Write-Host ""
Read-Host -Prompt "Press Enter to close... "
```

10. ❤️ Additionally give the shortcut a nice icon ❤️

💡 On Windows 10, by default the created shortcut will not have the black PowerShell 7 icon but an other ugly one, you can assign the correct one like this (or the Git one).

Right click on shortcut > Properties > Change icon
Icons paths:

```shell
C:\Program Files\WindowsApps\Microsoft.PowerShell_7.5.4.0_x64__8wekyb3d8bbwe\pwsh.exe
```

```shell
C:\Program Files\Git\git-bash.exe
```

### Setup for all OS start here

1. ⚠️ I you don't use a personal token to request the Github API this script will not work. To set up an identification token on the Github API and environements variables, go lower...

![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/git_pull_script.png)

Request the github api with a personnal token increase the rate limit and allow you to update a private repository...

- On github.com:

Settings > Developer settings > Personal access tokens > Tokens (classic) > Generate new token > Generate new token (classic)

- Input "Note": Your token name...
- Expiration option: "No expiration"
- Tick checkbox: "repo"
- Click on "Generate token"

⚠️ Be careful to copy your token because it will no longer be visible afterwards!

### On windows

Setup your username and token in the environment variables.
![First Step](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/git_pull_script_config_environement_variable_step_1.png)  

![Second Step](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/git_pull_script_config_environement_variable_step_2.png)

Repeat operation for the username...  

### On Linux / macOS

Open your PowerShell profile with your favorite editor.  

```powershell
nano $PROFILE
```

Add these two lines at the top of the file (replace with your real credentials) :  

```powershell
$env:GITHUB_TOKEN = "ghp_YourGeneratedTokenHere..."
$env:GITHUB_USERNAME = "YourGitHubUsername"
```

Save and exit, reload your profile now :  

```powershell
. $PROFILE
```

🔒 **Security Tip :**  
Since this file contains a secret token, it is recommended to restrict read permissions to your user only.  

```powershell
chmod 600 $PROFILE
```

2. Now you must open your "Microsoft.PowerShell_profile.ps1" file in your favorite text editor.

3. Copy/Paste "Update-GitRepositories" function and his utilities functions inside.

```powershell
#--------------------------------------------------------------------------#
#                              ALIASES                                     #
#--------------------------------------------------------------------------#

Set-Alias -Name gpull -Value Update-GitRepositories


#-----------------------------------------------------------------------#
#                        GLOBAL VARIABLES                               #
#-----------------------------------------------------------------------#

$Global:TerminalWidth = 100


#---------------------------------------------------------------------------#
#                        LOCATION PATH CONFIG                               #
#---------------------------------------------------------------------------#

function Get-LocationPathConfig {
  # IsRepo = $true   => Included in Update-GitRepositories() process AND accessible via go()
  # IsRepo = $false  => Accessible ONLY via go() function

  # IsOnlyMain = $true   => only pull main branch
  # IsOnlyMain = $false  => pull all branches

  # Get system context
  $Sys = Get-SystemContext

  # Definition of universal root folders
  # Join-Path automatically handles "/"" or "\"" depending on specific OS
  $DesktopPath   = Join-Path $HOME "Desktop"
  $ProjectsPath  = Join-Path $DesktopPath "Projets"
  $DocumentsPath = Join-Path $HOME "Documents"
  $PicturesPath  = Join-Path $HOME "Pictures"

  # For nvim, path changes depending on OS
  if ($Sys.IsMacOS -or $Sys.IsLinux) {
    $NvimPath = Join-Path $HOME ".config/nvim"
  }
  else {
    $NvimPath = Join-Path $env:LOCALAPPDATA "nvim"
  }

  return @(
    ##########---------- REPOSITORIES (Important order for Update-GitRepositories() function) ----------##########
    [PSCustomObject]@{ Name = "AngularTemplate";          Path = Join-Path $ProjectsPath   "AngularTemplate";                  IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "ArtiWave";                 Path = Join-Path $ProjectsPath   "ArtiWave";                         IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "Astrofalls";               Path = Join-Path $ProjectsPath   "Astrofalls";                       IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "CotonShop";                Path = Join-Path $ProjectsPath   "CotonShop";                        IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "Cours";                    Path = Join-Path $DesktopPath    "Cours";                            IsRepo = $true;     IsOnlyMain = $true  },
    [PSCustomObject]@{ Name = "DailyPush";                Path = Join-Path $DesktopPath    "DailyPush";                        IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "DataScrub";                Path = Join-Path $ProjectsPath   "DataScrub";                        IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "Documentations";           Path = Join-Path $DocumentsPath  "Documentations";                   IsRepo = $true;     IsOnlyMain = $true  },
    [PSCustomObject]@{ Name = "Dotfiles";                 Path = Join-Path $DesktopPath    "Dotfiles";                         IsRepo = $true;     IsOnlyMain = $true  },
    [PSCustomObject]@{ Name = "EasyGarden";               Path = Join-Path $ProjectsPath   "EasyGarden";                       IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "ElexxionData";             Path = Join-Path $ProjectsPath   "ElexxionData";                     IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "ElexxionMinio";            Path = Join-Path $ProjectsPath   "ElexxionMinio";                    IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "EmmanuelLefevre";          Path = Join-Path $ProjectsPath   "EmmanuelLefevre";                  IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "GestForm";                 Path = Join-Path $ProjectsPath   "GestForm";                         IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "GitHubProfileIcons";       Path = Join-Path $PicturesPath   "GitHubProfileIcons";               IsRepo = $true;     IsOnlyMain = $true  },
    [PSCustomObject]@{ Name = "GoogleSheets";             Path = Join-Path $DesktopPath    "GoogleSheets";                     IsRepo = $true;     IsOnlyMain = $true  },
    [PSCustomObject]@{ Name = "LeCabinetDeCuriosites";    Path = Join-Path $ProjectsPath   "LeCabinetDeCuriosites";            IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "IAmEmmanuelLefevre";       Path = Join-Path $ProjectsPath   "IAmEmmanuelLefevre";               IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "MarkdownImg";              Path = Join-Path $DesktopPath    "MarkdownImg";                      IsRepo = $true;     IsOnlyMain = $true  },
    [PSCustomObject]@{ Name = "Mflix";                    Path = Join-Path $ProjectsPath   "Mflix";                            IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "OmbreArcane";              Path = Join-Path $ProjectsPath   "OmbreArcane";                      IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "OpenScraper";              Path = Join-Path $ProjectsPath   "OpenScraper";                      IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "ParquetFlow";              Path = Join-Path $ProjectsPath   "ParquetFlow";                      IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "ReplicaMySQL";             Path = Join-Path $ProjectsPath   "ReplicaMySQL";                     IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "Schemas";                  Path = Join-Path $DesktopPath    "Schemas";                          IsRepo = $true;     IsOnlyMain = $true  },
    [PSCustomObject]@{ Name = "ScrapMate";                Path = Join-Path $ProjectsPath   "ScrapMate";                        IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "Sortify";                  Path = Join-Path $ProjectsPath   "Sortify";                          IsRepo = $true;     IsOnlyMain = $false },
    [PSCustomObject]@{ Name = "Soutenances";              Path = Join-Path $DesktopPath    "Soutenances";                      IsRepo = $true;     IsOnlyMain = $true  },
    [PSCustomObject]@{ Name = "Yam4";                     Path = Join-Path $ProjectsPath   "Yam4";                             IsRepo = $true;     IsOnlyMain = $false },

    ##########---------- NAVIGATION ONLY (go() function) ----------##########
    [PSCustomObject]@{ Name = "desktop";                  Path = $DesktopPath;                                                IsRepo = $false },
    [PSCustomObject]@{ Name = "dwld";                     Path = Join-Path $HOME "Downloads";                                 IsRepo = $false },
    [PSCustomObject]@{ Name = "home";                     Path = $HOME;                                                       IsRepo = $false },
    [PSCustomObject]@{ Name = "nvim";                     Path = $NvimPath;                                                   IsRepo = $false },
    [PSCustomObject]@{ Name = "profile";                  Path = Split-Path $PROFILE;                                         IsRepo = $false },
    [PSCustomObject]@{ Name = "projets";                  Path = $ProjectsPath;                                               IsRepo = $false }
  )
}


#-----------------------------------------------------------------------#
#                        SHARED FUNCTIONS                               #
#-----------------------------------------------------------------------#

##########---------- Get system environment context ----------##########
function Get-SystemContext {
  # PowerShell editing detection (the engine)
  # Desktop = PowerShell 5.1 (old Windows)
  # Core    = PowerShell 7+ (modern : Windows, Linux, Mac)
  $IsCore = $PSVersionTable.PSEdition -ne 'Desktop'

  # OS detection
  # In older PowerShell 5.1 versions, variables $IsLinux/$IsMacOS not exist => assume Windows
  # Also, use different names ($detect...) to avoid conflicts with system reserved variables
  $detectLinux   = $false
  $detectMacOS   = $false
  $detectWindows = $true

  if ($IsCore) {
    # In PowerShell we simply read native variables
    # If $IsLinux exists and is true, we update our internal variable
    if ($IsLinux) { $detectLinux = $true; $detectWindows = $false }
    if ($IsMacOS) { $detectMacOS = $true; $detectWindows = $false }
  }

  # Clean/easy-to-use item
  return [PSCustomObject]@{
    IsCore    = $IsCore
    IsDesktop = -not $IsCore
    IsLinux   = $detectLinux
    IsMacOS   = $detectMacOS
    IsWindows = $detectWindows
  }
}

##########---------- Check if Git is installed and available ----------##########
function Test-GitAvailability {
  param (
    # Default message
    [string]$Message = "⛔ Git is not installed (or not found in path)... Install it before using this command ! ⛔",

    # By default text is centered
    [bool]$Center = $true
  )

  # Check command existence
  if (Get-Command git -ErrorAction SilentlyContinue) {
    return $true
  }

  # Display Logic
  if ($Center) {
    Show-GracefulError -Message $Message
  }
  else {
    Show-GracefulError -Message $Message -NoCenter
  }

  return $false
}

##########---------- Calculate centered padding spaces ----------##########
function Get-CenteredPadding {
  param (
    [int]$TotalWidth = $Global:TerminalWidth,
    [string]$RawMessage
  )

  # Removes invisible characters from "Variation Selector"
  $cleanMsg = $RawMessage -replace "\uFE0F", ""

  # Standard length in memory
  $visualLength = $cleanMsg.Length

  # If message contains "simple" BMP emojis (one character long but two characters wide on screen), we add 1
  $bmpEmojis = ([regex]::Matches($cleanMsg, "[\u2300-\u23FF\u2600-\u27BF]")).Count
  $visualLength += $bmpEmojis

  # (Total Width - Message Length) / 2
  # [math]::Max(0, ...) => prevents crash if message is longer than $Global:TerminalWidth characters
  $paddingCount = [math]::Max(0, [int](($TotalWidth - $visualLength) / 2))

  return " " * $paddingCount
}

##########---------- Display a frame header ----------##########
function Show-HeaderFrame {
  param (
    [Parameter(Mandatory=$true)]
    [string]$Title,

    [ConsoleColor]$Color = "Cyan"
  )

  # Fixed constraints
  $TerminalWidth = $Global:TerminalWidth
  $FrameWidth = 64
  $FramePaddingLeft = ($TerminalWidth - $FrameWidth) / 2

  # Frame margins
  $leftMargin = " " * $FramePaddingLeft

  ######## INTERN CONTENT ########
  # Space around title inside frame
  $middleContentRaw = " $Title "

  # Length of horizontal bar between borders ╔ and ╗
  $horizontalBarLength = $FrameWidth - 2

  # Title length
  $TitleLength = $middleContentRaw.Length

  # Total space available to center title
  $TotalInternalSpace = $horizontalBarLength - $TitleLength

  # Internal margin to center title (in 62 characters)
  $InternalLeftSpaces = [System.Math]::Floor($TotalInternalSpace / 2)

  if ($InternalLeftSpaces -lt 0) {
    $InternalLeftSpaces = 0
  }

  $InternalLeftMargin = " " * $InternalLeftSpaces

  # Title with internal left padding
  $PaddedTitle = $InternalLeftMargin + $middleContentRaw

  # Fill in remaining space
  $PaddedTitle += " " * ($horizontalBarLength - $PaddedTitle.Length)

  # Create 62-character border bar
  $horizontalBar = "═" * $horizontalBarLength

  # Display frame header
  Write-Host ""
  Write-Host "$leftMargin╔$horizontalBar╗" -ForegroundColor $Color
  Write-Host "$leftMargin║$PaddedTitle║" -ForegroundColor $Color
  Write-Host "$leftMargin╚$horizontalBar╝" -ForegroundColor $Color
  Write-Host ""
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
  $totalWidth = $Global:TerminalWidth
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

##########---------- Display technical error message ----------##########
function Show-TechnicalErrorDetail {
  param (
    [Parameter(Mandatory=$true)]
    [string]$Message
  )

  Write-Host "Error message => " -ForegroundColor Red
  Write-Host $Message -ForegroundColor DarkBlue
}

##########---------- Display error message nicely ----------##########
function Show-GracefulError {
  param (
    [Parameter(Mandatory=$true)]
    [string]$Message,

    [Parameter(Mandatory=$false)]
    [System.Management.Automation.ErrorRecord]$ErrorDetails,

    [switch]$NoCenter,

    [switch]$NoTrailingNewline
  )

  if ($NoCenter) {
    Write-Host $Message -ForegroundColor Red
  }
  else {
    $paddingStr = Get-CenteredPadding -RawMessage $Message
    Write-Host -NoNewline $paddingStr
    Write-Host $Message -ForegroundColor Red
  }

  # Display technical details if provided
  if ($ErrorDetails) {
    Write-Host "$ErrorDetails" -ForegroundColor DarkBlue
  }

  # Adding final line break if requested
  if (-not $NoTrailingNewline) {
    Write-Host ""
  }
}


#--------------------------------------------------------------------------#
#                   UPDATE YOUR LOCAL REPOSITORIES                         #
#--------------------------------------------------------------------------#

function Update-GitRepositories {
  [CmdletBinding()]
  param (
    # Force repository information reloading
    [switch]$RefreshCache,
    # Optional parameter to update only one repository
    [string]$Name
  )

  if ($Name) {
    Show-HeaderFrame -Title "UPDATING LOCAL REPOSITORY"
  }
  else {
    Show-HeaderFrame -Title "UPDATING LOCAL REPOSITORIES"
  }

  ######## GUARD CLAUSE : GIT AVAILABILITY ########
  if (-not (Test-GitAvailability)) {
    return
  }

  ######## START GLOBAL TIMER ########
  $globalTimer = Start-OperationTimer

  ######## CACHE MANAGEMENT ########
  # If cache doesn't exist or if a refresh is forced
  if (-not $Global:GitReposCache -or $RefreshCache) {
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
    $Global:GitReposCache = @{
      ReposInfo = $tempReposInfo
    }
  }

  ######## DATA RETRIEVAL ########
  # Retrieve repositories information from cache
  $reposInfo  = $Global:GitReposCache.ReposInfo

  $reposOrder    = $reposInfo.Order
  $repos         = $reposInfo.Paths
  $username      = $reposInfo.Username
  $token         = $reposInfo.Token
  $reposSettings = $reposInfo.Settings

  ######## LAUNCH SCRIPT FOR A SINGLE REPO ########
  $reposToProcess = Get-RepoListToProcess -FullList $reposOrder -TargetName $Name
  # If helper returns `$null` (repo not found), we stop everything
  if ($null -eq $reposToProcess) {
    return
  }

  # Flag initialization
  $isFirstRepo = $true

  # Init summary table list
  $summaryTableList = @()

  ######## REPOSITORY ITERATION ########
  # Iterate over each repository in the defined order
  foreach ($repoName in $reposToProcess) {
    ######## START REPOSITORY TIMER ########
    $repoTimer = Start-OperationTimer

    ######## UPDATING SUMMARY TABLE STATUS ########
    $summaryTableCurrentStatus = "Skipped"

    try {
      ######## DATA SETUP ########
      $repoPath = $repos[$repoName]

      ######## UI : SEPARATOR MANAGEMENT ########
      # Display main separator (except first)
      if (-not $isFirstRepo) {
        Show-MainSeparator
      }
      # Mark first loop is finished
      $isFirstRepo = $false

      ######## GUARD CLAUSE : PATH EXISTS / CLONE PROPOSAL ########
      # Use -Silent to avoid error message, treating absence as an opportunity
      if (-not (Test-LocalRepoExists -Path $repoPath -Name $repoName -Silent)) {

        # Call clone manager
        $cloneStatus = Invoke-InteractiveClone -RepoName $repoName -RepoPath $repoPath -UserName $username

        # Dispatch result
        if ($cloneStatus -eq 'Success') {
          $summaryTableCurrentStatus = "✨ Cloned"

          # Skip rest of loop (pull/cleanup) => fresh clone is already perfect
          continue
        }
        elseif ($cloneStatus -eq 'Skipped') {
          $summaryTableCurrentStatus = "Ignored"

          continue
        }
        else {
          $summaryTableCurrentStatus = "Failed"

          continue
        }
      }

      ######## GUARD CLAUSE : NOT A GIT REPO ########
      if (-not (Test-IsGitRepository -Path $repoPath -Name $repoName)) {
        $summaryTableCurrentStatus = "Ignored"

        # Go finally block (and after next repository)
        continue
      }

      # Change current directory to repository path
      Set-Location -Path $repoPath

      ######## GUARD CLAUSE : REMOTE MISMATCH ########
      # Check local remote matches GitHub info
      if (-not (Test-LocalRemoteMatch -UserName $username -RepoName $repoName)) {
        $summaryTableCurrentStatus = "Ignored"

        # Go finally block (and after next repository)
        continue
      }

      ######## MAIN PROCESS ########
      # Show repository name being updated
      Write-Host -NoNewline "🚀 "
      Write-Host -NoNewline "$repoName" -ForegroundColor white -BackgroundColor DarkBlue
      Write-Host " is on update process..." -ForegroundColor Green

      ######## API CALL ########
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
        $summaryTableCurrentStatus = "Failed"

        # Go finally block (and after next repository)
        continue
      }

      ######## DATA RETRIEVAL : TRACKED BRANCHES ########
      # Find all local branches that have a remote upstream
      $branchesToUpdate = @(Get-LocalBranchesWithUpstream)

      ######## LOGIC : ONLY MAIN FILTER ########
      # Check if specific repo is configured to pull only main/master
      if ($reposSettings[$repoName] -eq $true) {
        # Keep only branches strictly named "main" or "master"
        $branchesToUpdate = @($branchesToUpdate | Where-Object { $_.Local -match '^(main|master)$' })

        if ($branchesToUpdate) {
          $msgPrefix = "ℹ️ Configured to pull "
          $msgBranch = "MAIN"
          $msgSuffix = " branch only ℹ️"

          $fullMsg = $msgPrefix + $msgBranch + $msgSuffix

          Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
          Write-Host -NoNewline $msgPrefix -ForegroundColor DarkYellow
          Write-Host -NoNewline $msgBranch -ForegroundColor Magenta
          Write-Host -NoNewline $msgSuffix -ForegroundColor DarkYellow
          Write-Host ""
        }
      }

      ######## GUARD CLAUSE : NO UPSTREAM ########
      # If no branch has an upstream defined, nothing to update or clean up
      if (-not $branchesToUpdate) {
        Write-Host "ℹ️ No upstream defined ! Nothing to update or clean up for this repository ! ℹ️" -ForegroundColor DarkYellow

        $summaryTableCurrentStatus = "Ignored"

        # Go finally block (and after next repository)
        continue
      }

      ######## BRANCH PROCESSING : SORTING ########
      # Organize branches : Main -> Dev -> Others by alphabetical
      $sortedBranchesToUpdate = Get-SortedBranches -Branches $branchesToUpdate

      # Track repository state
      $repoIsInSafeState = $true

      # Track if any branch needed a pull
      $anyBranchNeededPull = $false

      # Track if ALL branches were processed successfully (no skip, no fail)
      $allBranchesWerePulled = $true

      # Separator control
      $branchCount = $sortedBranchesToUpdate.Count
      $i = 0

      # Assume up-to-date until proven otherwise
      $summaryTableCurrentStatus = "Already-Updated"

      ######## UPDATE LOOP ########
      # Iterate over each branch found to pull updates from remote
      foreach ($branch in $sortedBranchesToUpdate) {
        # Increment iteration counter
        $i++

        ######## GUARD CLAUSE : CHECKOUT ########
        if (-not (Invoke-SafeCheckout -TargetBranch $branch.Local`
                                      -OriginalBranch $originalBranch)) {
          $repoIsInSafeState = $false
          $summaryTableCurrentStatus = "Failed"
          $allBranchesWerePulled = $false

          # Stop processing branches for this repo (fatal error)
          break
        }

        ######## STRATEGY : BOT BRANCHES (OUTPUT) ########
        $botSyncStatus = Invoke-BotBranchSync -BranchName $branch.Local

        if ($botSyncStatus -ne 'NotBot') {
          if ($botSyncStatus -eq 'Updated') {
            $summaryTableCurrentStatus = "Updated"
          }
          else {
            $summaryTableCurrentStatus = "Failed"
            $allBranchesWerePulled = $false
          }

          # If not last branch, display separator
          if ($i -lt $branchCount) {
            Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
          }

          # Jump to next branch in list
          continue
        }

        ######## GUARD CLAUSE : LOCAL CONFLICTS ########
        if (-not (Test-WorkingTreeClean -BranchName $branch.Local)) {
          if ($summaryTableCurrentStatus -ne "Failed") {
            $summaryTableCurrentStatus = "Skipped"
          }

          $allBranchesWerePulled = $false

          # Jump to next branch in list
          continue
        }

        ######## GUARD CLAUSE : UNPUSHED COMMITS ########
        if (Test-UnpushedCommits -BranchName $branch.Local) {
          if ($summaryTableCurrentStatus -ne "Failed") {
            $summaryTableCurrentStatus = "Skipped"
          }

          $allBranchesWerePulled = $false

          # Jump to next branch in list
          continue
        }

        ######## GUARD CLAUSE : ALREADY UPDATED ########
        if (Test-IsUpToDate -LocalBranch $branch.Local `
                            -RemoteBranch $branch.Remote) {

          # If not last branch, display separator
          if ($i -lt $branchCount) {
            Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
          }

          # Jump to next branch in list
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
        # Update was successfull
        if ($updateStatus -eq 'Success') {
          if ($summaryTableCurrentStatus -ne "Failed" -and $summaryTableCurrentStatus -ne "Skipped") {
            $summaryTableCurrentStatus = "Updated"
          }
        }
        # Update failed
        elseif ($updateStatus -eq 'Failed') {
          $summaryTableCurrentStatus = "Failed"
          $repoIsInSafeState = $false
          $allBranchesWerePulled = $false

          # Stop processing branches for this repo
          break
        }
        # Update Skipped (User said No)
        elseif ($updateStatus -eq 'Skipped') {
          $allBranchesWerePulled = $false

          # Update status of summary table
          if ($summaryTableCurrentStatus -ne "Failed") {
            $summaryTableCurrentStatus = "Skipped"
          }
        }

        # If execution was successful (Success or Skipped) and not last one, display separator
        if ($updateStatus -ne 'Failed' -and $i -lt $branchCount) {
          Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
        }
      }

      ######## POST-UPDATE ACTIONS (only if safe) ########
      # We check this flag because if a checkout failed (break), we must NOT try to cleanup
      if ($repoIsInSafeState) {
        ######## STATUS REPORT ########
        # If no branch needed pull and there was more than one branch to check
        if (($anyBranchNeededPull -eq $false) -and
            ($sortedBranchesToUpdate.Count -gt 1) -and
            ($allBranchesWerePulled -eq $true)) {
          Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray

          Write-Host "All branches are being updated 🤙" -ForegroundColor Green
        }

        ######## DETECT NEW BRANCHES ########
        # Calculate which remote branches are missing locally
        $newBranchesToTrack = Get-NewRemoteBranches

        ######## USER PERMISSION TO PULL NEW BRANCHES ########
        if (Invoke-NewBranchTracking -NewBranches $newBranchesToTrack) {
          $summaryTableCurrentStatus = "Failed"
        }

        # Track whether user's branch has been deleted
        [bool]$originalBranchWasDeleted = $false

        ######## CLEANUP : ORPHANED BRANCHES ########
        # Ask to clean branches that no longer exist on remote
        $orphanResult = Invoke-OrphanedCleanup -OriginalBranch $originalBranch
        if ($orphanResult.OriginalDeleted) {
          $originalBranchWasDeleted = $true
        }
        if ($orphanResult.HasError) {
          $summaryTableCurrentStatus = "Failed"
        }

        ######## CLEANUP : MERGED BRANCHES ########
        # Ask to clean branches that are already merged into main/dev
        $mergedResult = Invoke-MergedCleanup -OriginalBranch $originalBranch
        if ($mergedResult.OriginalDeleted) {
          $originalBranchWasDeleted = $true
        }
        if ($mergedResult.HasError) {
          $summaryTableCurrentStatus = "Failed"
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
          Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
        }

        ######## WORKFLOW INFO ########
        Show-MergeAdvice

        ######## RETURN STRATEGY ########
        Restore-UserLocation -OriginalBranch $originalBranch `
                            -RepoIsSafe $repoIsInSafeState `
                            -OriginalWasDeleted $originalBranchWasDeleted
      }
    }
    catch {
      ######## UPDATING SUMMARY TABLE STATUS ########
      $summaryTableCurrentStatus = "Failed"

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
    finally {
      ######## STOP TIMER ########
      $repoTimer.Stop()
      $elapsed = $repoTimer.Elapsed

      ######## FORMAT TIME ########
      $timeForTable = ""
      # Less than a second (Milliseconds)
      if ($elapsed.TotalSeconds -lt 1) {
        $timeForTable = "$($elapsed.TotalMilliseconds.ToString("0"))ms"
      }
      # More than a minute (Minutes + Seconds)
      elseif ($elapsed.TotalMinutes -ge 1) {
        $timeForTable = "$($elapsed.ToString("mm'm 'ss's'"))"
      }
      # Between 1s and 59s (Seconds)
      else {
        $timeForTable = "$($elapsed.ToString("ss's'"))"
      }

      ######## ADD TO SUMMARY TABLE ########
      $summaryTableList += [PSCustomObject]@{ Repo=$repoName; Status=$summaryTableCurrentStatus; Time=$timeForTable }

      ######## STOP REPOSITORY TIMER & DISPLAY ########
      Stop-OperationTimer -Watch $repoTimer -RepoName $repoName

      ######## RETURN HOME DIRECTORY ########
      Set-Location -Path $HOME
    }
  }

  ######## STOP GLOBAL TIMER & DISPLAY SUMMARY TABLE ########
  if ($reposToProcess.Count -gt 1) {
    Show-UpdateSummary -ReportData $summaryTableList
    Stop-OperationTimer -Watch $globalTimer -IsGlobal
  }
}


#--------------------------------------------------------------------------#
#                        GIT PULL UTILITIES FUNCTIONS                      #
#--------------------------------------------------------------------------#

##########---------- Get local repositories information ----------##########
function Get-RepositoriesInfo {
  ######## ENVIRONMENTS VARIABLES DEFINITION ########
  # GitHub token
  $gitHubToken = $env:GITHUB_TOKEN
  # GitHub username
  $gitHubUsername = $env:GITHUB_USERNAME

  ######## WINDOWS ENVIRONMENTS VARIABLES MESSAGE CONFIG ########
  $envVarMessageTemplate = "Check {0} in Windows Environment Variables..."
  $functionNameMessage = "in Get-RepositoriesInfo !"

  ######## GUARD CLAUSE : MISSING USERNAME ########
  if ([string]::IsNullOrWhiteSpace($gitHubUsername)) {
    Show-GracefulError -Message "❌ GitHub username is missing or invalid ! ❌" -NoTrailingNewline

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
    Show-GracefulError -Message "❌ GitHub token is missing or invalid ! ❌" -NoTrailingNewline

    # Helper called to center info message nicely
    $infoMsg = "ℹ️ " + ($envVarMessageTemplate -f "'GITHUB_TOKEN'")
    $paddingInfoStr = Get-CenteredPadding -RawMessage $infoMsg

    # Display info message
    Write-Host -NoNewline $paddingInfoStr
    Write-Host $infoMsg -ForegroundColor DarkYellow

    return $null
  }

  ######## LOAD PATH LOCATION CONFIG ########
  $allConfig = Get-LocationPathConfig

  ######## GUARD CLAUSE : CONFIG RETURN NOTHING ########
  if (-not $allConfig) {
    Show-GracefulError -Message "❌ Critical Error : Get-LocationPathConfig returned no data ! ❌"
    return $null
  }

  ######## FILTER AND ORDER (CHECK IsRepo = true) ########
  $gitConfig = $allConfig | Where-Object { $_.IsRepo -eq $true }
  $reposOrder = @($gitConfig.Name)

  ######## GUARD CLAUSE : EMPTY ORDER LIST ########
  if (-not $reposOrder -or $reposOrder.Count -eq 0) {
    Show-GracefulError -Message "❌ Local array repo order is empty ! ❌" -NoTrailingNewline

    # Helper called to center info message nicely
    $infoMsg = "ℹ️ Define at least one repository $functionNameMessage (order array)"
    $paddingInfoStr = Get-CenteredPadding -RawMessage $infoMsg

    # Display info message
    Write-Host -NoNewline $paddingInfoStr
    Write-Host $infoMsg -ForegroundColor DarkYellow

    return $null
  }

  ######## PATH VALIDATION 1 : SYNTAX INTEGRITY CHECK (BLOCKING) ########
  # ONLY check if path string is empty
  $invalidSyntaxItems = $gitConfig | Where-Object {
    [string]::IsNullOrWhiteSpace($_.Path)
  }

  if ($invalidSyntaxItems) {
    Show-GracefulError -Message "❌ Local repositories array contains empty paths ! ❌" -NoTrailingNewline

    Write-Host ""
    foreach ($bad in $invalidSyntaxItems) {
      Write-Host -NoNewline " └─ Empty path for : " -ForegroundColor DarkYellow
      Write-Host "📦 $($bad.Name)" -ForegroundColor DarkCyan
    }
    Write-Host ""

    $infoMsg = "ℹ️ Ensure repositories array has valid paths $functionNameMessage"
    $paddingInfoStr = Get-CenteredPadding -RawMessage $infoMsg

    Write-Host -NoNewline $paddingInfoStr
    Write-Host $infoMsg -ForegroundColor DarkYellow

    return $null
  }

  ######## PATH VALIDATION 2 : DISK CHECK (NON-BLOCKING WARNING) ########
  # Simply informing user that folder doesn't exist,
  # but no return $null => allow script to purpose clone
  $missingFolders = $gitConfig | Where-Object {
    -not (Test-Path -Path $_.Path -ErrorAction SilentlyContinue)
  }

  if ($missingFolders) {

    Write-Host ""
    foreach ($miss in $missingFolders) {
      Write-Host -NoNewline " └─ Path not found on disk : " -ForegroundColor DarkYellow
      Write-Host " $($miss.Path)" -ForegroundColor DarkCyan
    }
    Write-Host ""
  }

  ######## ARRAY CONSTRUCTION ########
  $repos = @{}
  $reposSettings = @{}

  foreach ($item in $gitConfig) {
    $repos[$item.Name] = $item.Path

    ######## SECURITY ########
    # Optional handling : if property does not exist, force $false
    if ($item.PSObject.Properties.Match('IsOnlyMain').Count) {
      $reposSettings[$item.Name] = $item.IsOnlyMain
    }
    else {
      $reposSettings[$item.Name] = $false
    }
  }

  ######## RETURN SUCCESS ########
  # Helper called to center message nicely
  $msg = "✔️ GitHub and projects configuration are nicely set ✔️"
  $paddingStr = Get-CenteredPadding -RawMessage $msg

  # Display message
  Write-Host -NoNewline $paddingStr
  Write-Host $msg -ForegroundColor Green
  Show-Separator -Length $Global:TerminalWidth -ForegroundColor Cyan
  Write-Host ""

  return @{
    Username = $gitHubUsername
    Token = $gitHubToken
    Order = $reposOrder
    Paths = $repos
    Settings = $reposSettings
  }
}

##########---------- Filter repositories list (All vs Single) ----------##########
function Get-RepoListToProcess {
  param (
    [array]$FullList,
    [string]$TargetName
  )

  # No optional parameters, return full list
  if ([string]::IsNullOrWhiteSpace($TargetName)) {
    return $FullList
  }

  # Use Where-Object to find original entry in $FullList (which is $reposOrder)
  $OriginalRepoName = $FullList | Where-Object { $_ -ieq $TargetName } | Select-Object -First 1

  # Name specified, check if it exists (case-insensitive)
  if ($OriginalRepoName) {
    # Helper called to center message nicely
    $msg = "🔎 Pull targeted on single repository 🔎"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display message
    Write-Host -NoNewline $paddingStr
    Write-Host "🔎 Pull targeted on single repository 🔎" -ForegroundColor Cyan

    Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray

    # Returns repository name with original capitalization
    return @($OriginalRepoName)
  }

  # Name not found
  else {
    # Helper called to center message nicely
    $msg = "⚠️ Repository `"$($TargetName)`" not found in your configuration list ! ⚠️"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display message
    Write-Host -NoNewline $paddingStr
    Write-Host -NoNewline "⚠️ Repository " -ForegroundColor Red
    Write-Host -NoNewline "`"$TargetName`"" -ForegroundColor Magenta
    Write-Host " not found in your configuration list ! ⚠️" -ForegroundColor Red

    return $null
  }
}

##########---------- Check if remote repository exists on GitHub ----------##########
function Test-RemoteRepositoryExistence {
  param (
    [string]$RepoName,
    [string]$UserName,
    [string]$Token
  )

  Write-Host "🔎 Checking remote availability..." -ForegroundColor DarkBlue

  $apiUrl = "https://api.github.com/repos/$UserName/$RepoName"

  try {
    # Try to retrieve information from the repository. If 404, go to catch
    $null = Invoke-RestMethod -Uri $apiUrl -Method Get -Headers @{ Authorization = "Bearer $Token" } -ErrorAction Stop

    return $true
  }
  catch {
    Write-Host ""
    Write-Host -NoNewline "⛔ Remote repository " -ForegroundColor Red
    Write-Host -NoNewline "$UserName" -ForegroundColor Magenta
    Write-Host -NoNewline "/" -ForegroundColor Red
    Write-Host -NoNewline "$RepoName" -ForegroundColor Magenta
    Write-Host " does not exist on GitHub !" -ForegroundColor Red
    Write-Host "  => Please check spelling in Get-LocationPathConfig..." -ForegroundColor DarkYellow

    return $false
  }
}

##########---------- Propose to clone missing repository via SSH ----------##########
function Invoke-InteractiveClone {
  param (
    [string]$RepoName,
    [string]$RepoPath,
    [string]$UserName
  )

  # Helper called to center message nicely
  Write-Host ""
  $msgPrefix = "⚠️ Repository "
  $msgSuffix = " not found on disk ⚠️"

  $fullMsg = $msgPrefix + $RepoName + $msgSuffix

  Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
  Write-Host -NoNewline $msgPrefix -ForegroundColor DarkYellow
  Write-Host -NoNewline $RepoName -ForegroundColor White -BackgroundColor DarkBlue
  Write-Host $msgSuffix -ForegroundColor DarkYellow
  Write-Host ""

  Write-Host -NoNewline "Target path : " -ForegroundColor Cyan
  Write-Host " $RepoPath" -ForegroundColor DarkCyan

  ######## GUARD CLAUSE : CHECK REMOTE EXISTENCE ########
  # If remote doesn't exist, immediately exit
  if (-not (Test-RemoteRepositoryExistence -RepoName $RepoName -UserName $UserName -Token $Token)) {
    return 'Failed'
  }

  # Ask user permission
  Write-Host -NoNewline "🐑 Clone it via SSH ? (Y/n): " -ForegroundColor Magenta

  $confirm = Wait-ForUserConfirmation

  if ($confirm) {
    Write-Host -NoNewline "⏳ Cloning " -ForegroundColor DarkBlue
    Write-Host -NoNewline $RepoName -ForegroundColor White -BackgroundColor DarkBlue
    Write-Host "..." -ForegroundColor DarkBlue

    # Ensure parent directory exists
    $parentDir = Split-Path -Path $RepoPath -Parent
    if (-not (Test-Path $parentDir)) {
      New-Item -ItemType Directory -Force -Path $parentDir | Out-Null
    }

    # Construct SSH URL
    $sshUrl = "git@github.com:$UserName/$RepoName.git"

    # Execute Clone
    git clone $sshUrl $RepoPath

    if ($LASTEXITCODE -eq 0) {
      Write-Host -NoNewline "  => " -ForegroundColor Green
      Write-Host -NoNewline "$RepoName" -ForegroundColor Magenta
      Write-Host " successfully cloned ✅" -ForegroundColor Green

      return 'Success'
    }
    else {
      Write-Host "❌ Clone failed. Check your SSH keys or internet connection. ❌" -ForegroundColor Red

      return 'Failed'
    }
  }
  else {
    Write-Host "⏩ Clone skipped by user..." -ForegroundColor DarkYellow

    return 'Skipped'
  }
}

##########---------- Check if local repository path exists ----------##########
function Test-LocalRepoExists {
  param (
    [string]$Name,
    [string]$Path,
    [switch]$Silent
  )

  ######## CHECK ########
  # Check if path variable is defined AND if folder exists on disk
  if ($Path -and (Test-Path -Path $Path)) {
    return $true
  }

  ######## ERROR HANDLING ########
  # If silent mode is ON, just return false without drama
  if ($Silent) {
    return $false
  }

  # Otherwise (Standard Mode), display the big red error
  # Helper called to center message nicely
  $msg = "⚠️ Local repository path for $Name doesn't exist ⚠️"
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

    Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
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
    Show-GracefulError -Message "⚠️ 'Git fetch' failed ! Check your Git access credentials... ⚠️" -NoCenter
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

  # Ensure that $Branches is at least an empty array for filters
  if ($null -eq $Branches) { $Branches = @() }

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
  $otherList  = @($Branches | Where-Object { -not ($allPriorityNames -icontains $_.Local) } | Sort-Object Local)

  ######## MERGE & RETURN ########
  # Use comma operator to merge arrays, ensuring an output array
  return ,($mainList + $devList + $otherList)
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

    Write-Host -NoNewline "ℹ️ Blocked by local changes on " -ForegroundColor DarkYellow
    Write-Host -NoNewline "$OriginalBranch" -ForegroundColor Magenta
    Write-Host ". Halting updates for this repository !" -ForegroundColor DarkYellow

    return $false
  }

  ######## SUCCESS FEEDBACK ########
  Write-Host -NoNewline "Inspecting branch " -ForegroundColor Cyan
  Write-Host "$TargetBranch" -ForegroundColor Magenta

  return $true
}

##########---------- Force sync for bot-managed branches (e.g. output) ----------##########
function Invoke-BotBranchSync {
  param (
    [string]$BranchName
  )

  # Branches list managed by robots
  $botBranches = @("output")

  # If branch isn't in list, nothing is done
  if ($botBranches -notcontains $BranchName) {
    return 'NotBot'
  }

  # If it's a bot branch, we force synchronization.
  Write-Host "🤖 Bot branch detected. Forcing sync... " -ForegroundColor Magenta

  # We force a reset on the server version (upstream)
  git reset --hard "@{u}" *> $null

  if ($LASTEXITCODE -eq 0) {
    return 'Updated'
  }
  else {
    return 'Failed'
  }
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

  ######## GUARD CLAUSE : INVALID REFERENCES ########
  if (-not (git rev-parse --verify $LocalBranch 2>$null) -or
    -not (git rev-parse --verify $RemoteBranch 2>$null)) {
    Write-Host "⚠️ Unable to read local/remote references for $LocalBranch ! ⚠️" -ForegroundColor Red

    return 'Failed'
  }

  # Default state
  $pullStatus = 'Skipped'

  ######## STRATEGY : AUTO-UPDATE (Main/Master) ########
  if ($LocalBranch -eq "main" -or $LocalBranch -eq "master") {
    Write-Host "⏳ Updating main branch..." -ForegroundColor Magenta
    Show-LatestCommitsMessages -LocalBranch $LocalBranch -RemoteBranch $RemoteBranch -HideHashes

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
    Show-LatestCommitsMessages -LocalBranch $LocalBranch -RemoteBranch $RemoteBranch -HideHashes

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

    Show-LatestCommitsMessages -LocalBranch $LocalBranch -RemoteBranch $RemoteBranch -HideHashes

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
      Write-Host -NoNewline "  => " -ForegroundColor Green
      Write-Host -NoNewline "$LocalBranch" -ForegroundColor Magenta
      Write-Host " successfully updated ✅" -ForegroundColor Green

    }
    'Failed' {
      Write-Host "❌ "
      Write-Host -NoNewline "Error updating " -ForegroundColor Red
      Write-Host -NoNewline "$LocalBranch" -ForegroundColor Magenta
      Write-Host -NoNewline " in " -ForegroundColor Red
      Write-Host -NoNewline "$RepoName" -ForegroundColor white -BackgroundColor DarkBlue
      Write-Host " ❌" -ForegroundColor Red
    }
  }

  return $pullStatus
}

##########---------- Get and show latest commit message ----------##########
function Show-LatestCommitsMessages {
  param (
    [string]$LocalBranch,
    [string]$RemoteBranch,
    [switch]$HideHashes
  )

  ######## DATA RETRIEVAL ########
  # Get HASH HEAD
  $localHash  = git rev-parse $LocalBranch 2>$null
  $remoteHash = git rev-parse $RemoteBranch 2>$null

  ######## DATA ANALYSIS : ANCESTRY ########
  $isLocalBehind  = git merge-base --is-ancestor $localHash $remoteHash 2>$null
  $isRemoteBehind = git merge-base --is-ancestor $remoteHash $localHash 2>$null

  # Divergence detection
  if (-not $isLocalBehind -and -not $isRemoteBehind) {
    # Calculate exact difference
    $counts = git rev-list --left-right --count "$LocalBranch...$RemoteBranch" 2>$null
    $ahead = 0
    $behind = 0

    if ($counts -match '(\d+)\s+(\d+)') {
      $ahead  = $Matches[1]
      $behind = $Matches[2]
    }

    Write-Host "🔀 Diverged history detected !" -ForegroundColor DarkYellow
    Write-Host -NoNewline "   └─ Your branch is ahead by " -ForegroundColor Magenta
    Write-Host -NoNewline "$ahead" -ForegroundColor DarkCyan
    Write-Host -NoNewline " and behind by " -ForegroundColor Magenta
    Write-Host "$behind" -ForegroundColor DarkCyan
  }

  # Get new commits
  $raw = git log --pretty=format:"%s" --no-merges "$LocalBranch..$RemoteBranch" 2>$null

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

  ######## GUARD CLAUSE : SINGLE COMMIT ########
  # One commit
  if ($newCommits.Count -eq 1) {
    Write-Host "Commit message : " -ForegroundColor Magenta
    Write-Host "  - `"$newCommits[0]`"" -ForegroundColor Cyan
    return
  }

  ######## DISPLAY MULTIPLE COMMITS (PAGINATION LOGIC) ########
  # Several commits
  Write-Host "New commits received :" -ForegroundColor Magenta

  # Configuration
  $displayLimit = 5
  $totalCommits = $newCommits.Count

  # If total is less or equal to limit, we show everything normally
  if ($totalCommits -le $displayLimit) {
    foreach ($commit in $newCommits) {
      Write-Host "  - `"$commit`"" -ForegroundColor DarkCyan
    }
  }
  # If we have more than limit
  else {
    # Display first 5 commits
    for ($i = 0; $i -lt $displayLimit; $i++) {
      Write-Host "  - `"$($newCommits[$i])`"" -ForegroundColor DarkCyan
    }

    # Calculate remaining
    $remainingCount = $totalCommits - $displayLimit

    # Interactive prompt
    Write-Host -NoNewline "⚠️ $remainingCount" -ForegroundColor Red
    Write-Host -NoNewline " more commits ! Show all ? (Y/n): " -ForegroundColor DarkYellow

    # Helper called for a robust response
    $showAll = Wait-ForUserConfirmation

    if ($showAll) {
      # Display rest
      for ($i = $displayLimit; $i -lt $totalCommits; $i++) {
        Write-Host "  - `"$($newCommits[$i])`"" -ForegroundColor DarkCyan
      }
    }
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
    return $false
  }

  ######## CONFIGURATION ########
  # Branches that should NEVER be deleted remotely even if user doesn't track them
  $protectedBranches = @("dev", "develop", "main", "master")

  Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray

  # Flag initialization
  $hasError = $false
  $isFirstLoop = $true

  ######## USER INTERACTION LOOP ########
  foreach ($newBranchRef in $NewBranches) {
    # Extract local name (ex: origin/feature/x -> feature/x)
    $null = $newBranchRef -match '^[^/]+/(.+)$'
    $localBranchName = $Matches[1]

    ######## UI : SEPARATOR MANAGEMENT ########
    # Display separator (except first)
    if (-not $isFirstLoop) {
      Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
    }
    # Mark first loop is finished
    $isFirstLoop = $false

    # Display Branch Found
    Write-Host -NoNewline "❤️ New remote branche found => " -ForegroundColor Blue
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
      Write-Host "$localBranchName" -ForegroundColor Green

      # Create local branch tracking remote branch
      git branch --track --quiet $localBranchName $newBranchRef 2>$null

      # Check if branch creation worked
      if ($LASTEXITCODE -eq 0) {
        Write-Host -NoNewline "  => "
        Write-Host -NoNewline "$localBranchName" -ForegroundColor Red
        Write-Host " successfully pulled ✅" -ForegroundColor Green
      }
      # If branch creation failed
      else {
        Write-Host -NoNewline "$localBranchName" -ForegroundColor Red
        Write-Host "⚠️ New creation branch has failed ! ⚠️" -ForegroundColor Red

        $hasError = $true
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
          Write-Host "  🔥 Removal of $localBranchName..." -ForegroundColor Magenta

          git push origin --delete $localBranchName 2>&1 | Out-Null

          # Check if branch deletion worked
          if ($LASTEXITCODE -eq 0) {
            Write-Host -NoNewline "  => "
            Write-Host -NoNewline "$localBranchName" -ForegroundColor Magenta
            Write-Host " successfully deleted from server ✅" -ForegroundColor Green
          }
          # If branch deletion failed
          else {
            Write-Host -NoNewline "⚠️ Failed to delete " -ForegroundColor Red
            Write-Host "origin/$localBranchName ⚠️" -ForegroundColor Magenta

            $hasError = $true
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
  return $hasError
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
  $currentBranch = (git rev-parse --abbrev-ref HEAD).Trim()

  # Find branches marked as ': gone]' in git verbose output
  $orphanedBranches = git branch -vv | Select-String -Pattern '\[.*: gone\]' | ForEach-Object {
    $line = $_.Line.Trim()
    if ($line -match '^\*?\s*([\S]+)') {
      $Matches[1]
    }
  }

  # Filter, remove protected branches
  $branchesToClean = $orphanedBranches | Where-Object {
    (-not ($protectedBranches -icontains $_))
  }

  ######## GUARD CLAUSE : NOTHING TO CLEAN ########
  # Original branch was NOT deleted (or empty list)
  if (-not $branchesToClean -or $branchesToClean.Count -eq 0) {
    return [PSCustomObject]@{ OriginalDeleted = $false; HasError = $false }
  }

  Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray

  Write-Host "🧹 Cleaning up orphaned local branches..." -ForegroundColor DarkYellow


  # Flags initialization
  $originalWasDeleted = $false
  $hasError = $false
  $isFirstBranch = $true

  ######## INTERACTIVE CLEANUP LOOP ########
  foreach ($orphaned in $branchesToClean) {
    if (-not $isFirstBranch) {
      Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
    }

    # Refresh current branch status (in case we moved in previous loop)
    $realTimeCurrentBranch = (git rev-parse --abbrev-ref HEAD).Trim()

    if (-not $isFirstBranch) {
      Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
    }
    $isFirstBranch = $false

    ######## DEAD BRANCH ########
    if ($orphaned -eq $realTimeCurrentBranch) {
      Write-Host -NoNewline "👻 You are currently on the orphaned branch " -ForegroundColor Cyan
      Write-Host -NoNewline "`"$($orphaned)`"" -ForegroundColor Magenta
      Write-Host " ..." -ForegroundColor Cyan

      ######## GUARD CLAUSE : DIRTY WORKTREE ########
      if (-not (Test-WorkingTreeClean -BranchName $orphaned)) {
        Write-Host "⛔ Cannot switch branch automatically : uncommitted/unstagged changes detected ! ⛔" -ForegroundColor Red
        Write-Host "   └─> Skipping cleanup for this branch." -ForegroundColor DarkYellow

        continue
      }

      # Find safe branch
      $safeBranch = "develop"
      if (-not (git rev-parse --verify $safeBranch 2>$null)) { $safeBranch = "dev" }
      if (-not (git rev-parse --verify $safeBranch 2>$null)) { $safeBranch = "main" }
      if (-not (git rev-parse --verify $safeBranch 2>$null)) { $safeBranch = "master" }

      # Evacuate on safe branch
      Write-Host -NoNewline "🔄 Evacuating to " -ForegroundColor Cyan
      Write-Host -NoNewline "`"$($safeBranch)`"" -ForegroundColor Magenta
      Write-Host " to allow deletion 🔄" -ForegroundColor Cyan

      git checkout $safeBranch 2>$null | Out-Null

      if ($LASTEXITCODE -ne 0) {
        Write-Host -NoNewline "❌ Failed to checkout " -ForegroundColor Red
        Write-Host -NoNewline "`"$($safeBranch)`"" -ForegroundColor Magenta
        Write-Host -NoNewline ". Deletion aborted." -ForegroundColor Red

        $hasError = $true

        continue
      }
      # Success : no longer on branch, we can proceed to delete it
    }

    ######## STANDARD DELETION LOGIC ########
    # Helper called for warn about stash on branch
    Show-StashWarning -BranchName $orphaned

    # Ask user
    Write-Host -NoNewline "🗑️ Delete the orphaned local branch " -ForegroundColor Magenta
    Write-Host -NoNewline "$orphaned" -ForegroundColor Red
    Write-Host -NoNewline " ? (Y/n): " -ForegroundColor Magenta

    # Helper called for a robust response
    $wantToDelete = Wait-ForUserConfirmation

    if ($wantToDelete) {
      Write-Host "  🔥 Removal of $orphaned..." -ForegroundColor Magenta

      # Attempt secure removal
      git branch -d $orphaned *> $null

      # Check if deletion worked
      if ($LASTEXITCODE -eq 0) {
        Write-Host -NoNewline "  => "
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
            Write-Host -NoNewline "  => "
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

            $hasError = $true
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

  return [PSCustomObject]@{
    OriginalDeleted = $originalWasDeleted
    HasError        = $hasError
  }
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

  Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray

  Write-Host "🧹 Cleaning up local branches that have already being merged..." -ForegroundColor DarkYellow


  # Flags initialization
  $originalWasDeleted = $false
  $hasError = $false
  $isFirstBranch = $true

  ######## INTERACTIVE CLEANUP LOOP ########
  foreach ($merged in $branchesToClean) {
    if (-not $isFirstBranch) {
      Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkGray
    }

    $isFirstBranch = $false

    # Helper called for warn about stash on branch
    Show-StashWarning -BranchName $merged

    # Ask user
    Write-Host -NoNewline "Local branch " -ForegroundColor Magenta
    Write-Host -NoNewline "$merged" -ForegroundColor Red
    Write-Host -NoNewline " is already merged. 🗑️ Delete ? (Y/n): " -ForegroundColor Magenta

    # Helper called for a robust response
    $wantToDelete = Wait-ForUserConfirmation

    if ($wantToDelete) {
      Write-Host "  🔥 Removal of $merged..." -ForegroundColor Magenta

      # Secure removal (guaranteed to work because we checked --merged)
      git branch -D $merged *> $null

      # Check if deletion worked
      if ($LASTEXITCODE -eq 0) {
        Write-Host -NoNewline "  => "
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

        $hasError = $true
      }
    }
  }

  return [PSCustomObject]@{
    OriginalDeleted = $originalWasDeleted
    HasError        = $hasError
  }
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
    if ($DryRun) {
      return $false
    }

    return
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

  # Retrieves raw list as text
  $stashList = git stash list --no-color

  # Filter to keep only lines relevant to our branch
  $branchStashes = $stashList | Where-Object { $_ -match "On ${BranchName}:" }

  if ($branchStashes) {
    Write-Host -NoNewline "⚠️ WARNING : There are stashes on branch " -ForegroundColor Red
    Write-Host -NoNewline "$BranchName" -ForegroundColor Magenta
    Write-Host " ⚠️" -ForegroundColor Red

    foreach ($line in $branchStashes) {
      # Display stash line
      Write-Host "  - $line" -ForegroundColor DarkCyan

      # Extract stash Id with regex (ex: stash@{0})
      if ($line -match "stash@\{\d+\}") {
        $stashId = $matches[0].Trim()

        # Get files list in this stash
        $files = @(git stash show --name-only --include-untracked $stashId 2>&1)

        # If command fails (old Git), we try again without untracked option
        if ($LASTEXITCODE -ne 0) {
          $files = @(git stash show --name-only $stashId 2>&1)
        }

        if ($files.Count -gt 0) {
          foreach ($file in $files) {
            # Converts to string to be safe (in case it's an error object)
            $fileString = "$file".Trim()

            if (-not [string]::IsNullOrWhiteSpace($fileString)) {
              # Display files
              Write-Host "   $fileString" -ForegroundColor Gray
            }
          }
        }
      }
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
      Write-Host " doesn't exist ⚠️" -ForegroundColor Red
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
    Show-GracefulError -Message "💥 Internal Script/Git processing error 💥" -NoTrailingNewline

    # Debug message
    Show-TechnicalErrorDetail -Message $Message
  }
}

##########---------- Display update summary table ----------##########
function Show-UpdateSummary {
  param (
    [array]$ReportData
  )

  # If no data, no do anything
  if (-not $ReportData -or $ReportData.Count -eq 0) { return }

  Show-MainSeparator

  # Helper called to center table title nicely
  $title = "📊 UPDATE SUMMARY REPORT 📊"
  $padding = Get-CenteredPadding -RawMessage $title

  # Display table title
  Write-Host -NoNewline $padding
  Write-Host $title -ForegroundColor Cyan
  Write-Host ""


  ######## TABLE WIDTHS (fixed at 64 characters) ########
  $colRepoWidth = 24
  $colStatWidth = 24
  $colTimeWidth = 16

  $fixedTableWidth = $colRepoWidth + $colStatWidth + $colTimeWidth

  ######## TABLE CENTERING (Dynamic) ########
  $tableOuterPadding = " " * 8
  # Calculation of exterior padding based on overall width
  $outerPaddingLength = [math]::Max(0, [int](($Global:TerminalWidth - $fixedTableWidth) / 2))
  $tableOuterPadding = " " * $outerPaddingLength

  # Headers (manual format for color control)
  Write-Host -NoNewline $tableOuterPadding
  Write-Host -NoNewline "Repository              " -ForegroundColor White -BackgroundColor DarkGray
  Write-Host -NoNewline "        Status          " -ForegroundColor White -BackgroundColor DarkGray
  Write-Host -NoNewline "        Duration" -ForegroundColor White -BackgroundColor DarkGray
  Write-Host ""

  # Loop on results
  foreach ($item in $ReportData) {
    # Icon Definition + Color
    $statusText = $item.Status
    $statusColor = "White"

    switch ($item.Status) {
      "Already-Updated"   { $statusText = "✨ Already Updated";    $statusColor = "DarkCyan"   }
      "Cloned"            { $statusText = "🐙 Cloned";             $statusColor = "Cyan"       }
      "Failed"            { $statusText = "❌ Failed";             $statusColor = "Red"        }
      "Ignored"           { $statusText = "🙈 Ignored";            $statusColor = "Magenta"    }
      "Skipped"           { $statusText = "⏩ Skipped";            $statusColor = "DarkYellow" }
      "Updated"           { $statusText = "✅ Updated";            $statusColor = "Green"      }
    }

    ######## STATUS CENTERING ########
    $statLen = $statusText.Length
    # Manual adjustment for emojis that count double on screen
    if ($statusText -match "✅|✨|⏩|❌") {
      $statLen += 1
    }

    $padStatLeft = [math]::Max(0, [int](($colStatWidth - $statLen) / 2))
    $padStatStr = " " * $padStatLeft

    ######## OVERALL MARGIN ########
    Write-Host -NoNewline $tableOuterPadding

    ######## COLUMN 1 (Left aligned) ########
    Write-Host -NoNewline ("{0,-$colRepoWidth}" -f $item.Repo) -ForegroundColor Cyan

    ######## COLUMN 2 (Centered)) ########
    Write-Host -NoNewline $padStatStr
    Write-Host -NoNewline $statusText -ForegroundColor $statusColor

    # Calculating the remaining padding to align next column
    $padStatRightLen = $colStatWidth - $padStatLeft - $statLen
    if ($padStatRightLen -gt 0) {
      Write-Host -NoNewline (" " * $padStatRightLen)
    }

    ######## COLUMN 3 (Right align) ########
    $timeString = "{0,$colTimeWidth}" -f $item.Time
    Write-Host $timeString -ForegroundColor Magenta
  }
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
    Show-Separator -Length $Global:TerminalWidth -ForegroundColor DarkBlue

    # Helper called to center message nicely
    $msg = "⏱️ $RepoName updated in $timeString ⏱️"
    $paddingStr = Get-CenteredPadding -RawMessage $msg

    # Display repository timer
    Write-Host ""
    Write-Host -NoNewline $paddingStr
    Write-Host -NoNewline "⏱️ "
    Write-Host -NoNewline "$repoName" -ForegroundColor white -BackgroundColor DarkBlue
    Write-Host -NoNewline " updated in " -ForegroundColor Green
    Write-Host "$timeString ⏱️" -ForegroundColor Magenta
  }
}
```

4. Now you can use the function in your terminal by typing the alias and press ENTER :

```powershell
gpull
```

Or launch it with the desktop shortcut 🔥🔥🔥

## Bonus

🧠 You can easily launch script automatically at your computer starts 🧠

### Windows

**Win + R** -> type `shell:startup`  
Copy (Ctrl+C) your desktop shortcut and paste it in the "Getting Started" folder...  

Now the script will be launched every time you start your PC 💪

### Linux

Unlike Windows, Linux uses .desktop files for startup items.  

1. Create the autostart directory (if it doesn't exist).

```bash
mkdir -p ~/.config/autostart
```

2. Create the shortcut file.

```bash
nano ~/.config/autostart/git-auto-update.desktop
```

3. Paste this content inside : this configuration opens a terminal, loads your profile and runs the function.

```Ini, TOML
[Desktop Entry]
Type=Application
Name=Global Git Update
Comment=Update all git repositories on login
# Command: Launch PowerShell, Load Profile, Run Function, Wait for user
Exec=pwsh -Command "& { . $PROFILE; Update-GitRepositories; Read-Host 'Press Enter to close...' }"
Hidden=false
NoDisplay=false
X-GNOME-Autostart-enabled=true
Terminal=true
```

4. Save and Exit.

5. Restart your session.

Now the script will be launched every time you start your PC 💪

### macOS

macOS treats files with the .command extension as clickable shell scripts that automatically open the Terminal.  

1. Create the launcher file : open your terminal and create a file in your Documents folder (or anywhere you like).

```bash
nano ~/Documents/StartGitUpdate.command
```

2. Paste this content inside : this script invokes PowerShell, loads your profile (to get tokens and functions), runs the update, and waits for user input.

```bash
#!/bin/bash
echo "🚀 Starting Global Git Update..."
pwsh -Command "& { . $PROFILE; Update-GitRepositories; Read-Host 'Press Enter to close...' }"
```

3. Save and Exit.

4. Make it executable : this step is mandatory to allow macOS to run the file.

```bash
chmod +x ~/Documents/StartGitUpdate.command
```

5. Add to Login Items :  

- Open System Settings > General > Login Items
- Click the (+) button.
- Select your file ~/Documents/StartGitUpdate.command.

Now, every time you log in, a Terminal window will pop up, update your repos, and wait for you to check the green ticks ✅ before closing.

## Console Application Screens

![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/git_pull_script_2.png)
<br><br><br>
![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/git_pull_script_3.png)

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
