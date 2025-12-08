# POWERSHELL GLOBAL GIT IGNORE MANAGER SCRIPT

## SOMMAIRE

- [INTRO](#intro)
- [PRESENTATION](#presentation)
- [WORKFLOW](#workflow)
- [INSTALLATION PROCEDURE](#installation-procedure)
- [BONUS](#bonus)
- [CONSOLE APPLICATION SCREENS](#console-application-screens)

## INTRO

This documentation details the architecture of the Global GitIgnore Manager, a PowerShell automation module designed to standardize, maintain, and safeguard your global git exclusion rules across all your development environments, automatically at startup.

## PRESENTATION

😫 The Problem =>  
🚀 Stop polluting your repositories with .DS_Store, Thumbs.db or IDE config files ! 🚀  

Maintaining a consistent **.gitignore_global** file across multiple machines is a chore. Often, we add a rule for a specific language but forget it on the next laptop. Even worse, automated tools often overwrite your personal, specific exclusions.  

I decided to automate this with Set-LoadGlobalGitIgnore : a state-manager script acting as a "Standard of Truth" while strictly respecting your personal customizations.

🏗 **ARCHITECTURE**  
Hybrid Parser & Builder based on a "Sandwich" logic :

- Standard Template
- Dynamic Updates
- User Customizations

🧠 **PHILOSOPHY**

- Idempotency (0ms impact if nothing changed)
- Non-Destructive (User Customizations are sacred)
- Safety (Backup driven)
- UX

⚡ **TRIGGER**

- Event-Driven (startup terminal)
- CLI (aka Powershell: Set-LoadGlobalGitIgnore)

🛠️ **FEATURES**

1. 🧩 **Smart Merging Strategy (The Sandwich)**

- Standard Enforcement, ensures all rules defined in the script's template are present
- Preservation, automatically detects # USER CUSTOMIZATIONS section and strictly preserves everything below
- Dynamic Injection, injects missing rules into a dedicated # NEW IGNORE RULES block at the end of the file

2. 🧹 **Auto-Formatting & Hygiene**

- Category Grouping, parses comments to group rules logically
- Alphabetical Sorting, automatically sorts rules within their categories for better readability
- Comment Formatting, capitalizes and spaces comments (ex: transforms #test into # Test)
- Clean Structure, ensures proper spacing between sections to avoid "wall of text" files

3. 🛡️ **Safety & Integrity (Crash Proof)**

- Atomic Operation, creates a .gitignore_global.bak copy before modifying a single line
- Crash Recovery, preserves the backup and displays a visual warning path if the script fails
- Auto-Cleanup, silently removes the backup file upon successful update

4. 🧠 **Intelligent Parsing**

- Diff Detection, calculates exactly which rules are missing compared to the existing file
- Context Awareness, avoiding duplication
- Section Logic, completely rebuilds the # NEW IGNORE RULES section

5. ⚓ **Git Configuration Enforcement**

- Auto-Config, checks and sets git config --global core.excludesfile if needed
- Path Normalization, handles Windows (\) vs Git (/) path separators to avoid false positives
- Self-Healing, automatically repairs the config if it points to a wrong or missing file

6. 📈 **Visual Feedback**

- Status Reporting
- Granular Logs, displays exactly which rules were added ( .DS_Store)
- Error Handling, uses shared Show-GracefulError functions for consistent error reporting

👌 Others many controls and features have been added 👌

## WORKFLOW

1. **Define the template :** The Get-DefaultGlobalGitIgnoreTemplate function holds the source of truth (OS files, IDEs, Languages...)

2. **Startup Check :** On terminal launch, Set-LoadGlobalGitIgnore is called

3. **Analysis :**

- It reads the existing .gitignore_global
- It strips out the dynamically generated section
- It compares the rest with the template

4. **Actions :**

- If differences are found -> Backup -> Rebuild File -> Save -> Delete Backup
- If identical -> Do Nothing

## INSTALLATION PROCEDURE

1. You must open your "Microsoft.PowerShell_profile.ps1" file with your favorite text editor.

2. Copy/Paste "Set-LoadGlobalGitIgnore" function and his utilities functions inside.

```powershell
#-----------------------------------------------------------------------#
#                        SHARED FUNCTIONS                               #
#-----------------------------------------------------------------------#

##########---------- Check if Git is installed and available ----------##########
function Test-GitAvailability {
  param (
    # Default message
    [string]$Message = "⛔ Git for Windows is not installed (or not found in path)... Install it before using this command ! ⛔",

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
    [int]$TotalWidth = 80,
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
  # [math]::Max(0, ...) => prevents crash if message is longer than 80 characters
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
  $TerminalWidth = 80
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


#---------------------------------------------------------------------#
#                   GLOBAL GIT IGNORE MANAGER                         #
#---------------------------------------------------------------------#

function Set-LoadGlobalGitIgnore {
  $GitGlobalIgnorePath = Join-Path -Path $HOME -ChildPath ".gitignore_global"

  # Flag created or updated
  $WasUpdatedOrCreated = $false

  ######## GUARD CLAUSE : GIT AVAILABILITY ########
  if (-not (Test-GitAvailability -Message "⛔ Git for Windows is not installed (or not found in path). Global git ignore config skipped ! ⛔")) {
    return
  }

  # Load default template content
  $DefaultLines = Get-DefaultGlobalGitIgnoreTemplate

  ######## FILE CREATION/UPDATE ORCHESTRATION ########
  if (-not (Test-Path $GitGlobalIgnorePath)) {
    ######## CASE 1 : FILE DOESN'T EXIST (CREATION) ########
    Initialize-GlobalGitIgnoreFile -Path $GitGlobalIgnorePath -ContentLines $DefaultLines

    $WasUpdatedOrCreated = $true
  }
  ######## CASE 2 : FILE EXIST (UPDATE) ########
  else {
    $WasUpdatedOrCreated = Update-GlobalGitIgnoreFile -Path $GitGlobalIgnorePath -DefaultLines $DefaultLines
  }

  ######## GIT CONFIGURATION ########
  # Only display if file was touched or config is wrong
  Set-GlobalGitIgnoreReference -Path $GitGlobalIgnorePath -ShowMessage $WasUpdatedOrCreated
}


#--------------------------------------------------------------------------#
#                   GLOBAL GIT IGNORE UTILITIES FUNCTIONS                  #
#--------------------------------------------------------------------------#

##########---------- Initialize .gitignore_global file if missing ----------##########
function Initialize-GlobalGitIgnoreFile {
  param (
    [string]$Path,
    [string[]]$ContentLines
  )

  Show-HeaderFrame -Title "LOAD GLOBAL GIT IGNORE CONFIGURATION"

  # Message 1 : Not Found
  $msgPrefix = " .gitignore_global"
  $msgSuffix = " not found..."

  $fullMsg = $msgPrefix + $msgSuffix

  Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
  Write-Host -NoNewline $msgPrefix -ForegroundColor Cyan
  Write-Host $msgSuffix -ForegroundColor DarkYellow
  Write-Host ""

  # Message 2 : Creating
  $msg = "🔄 Creating it with default template 🔄"

  Write-Host -NoNewline (Get-CenteredPadding -RawMessage $msg)
  Write-Host $msg -ForegroundColor Red

  try {
    $ContentLines | Set-Content -Path $Path -Encoding UTF8 -Force

    $msg = "✅ File created successfully ✅"

    Write-Host -NoNewline (Get-CenteredPadding -RawMessage $msg)
    Write-Host $msg -ForegroundColor Green
    Write-Host ""
  }
  catch {
    Show-GracefulError -Message "❌ Error creating default template : " -NoCenter -ErrorDetails $_
  }
}

##########---------- Update .gitignore_global file if exists ----------##########
function Update-GlobalGitIgnoreFile {
  param (
    [string]$Path,
    [string[]]$DefaultLines
  )

  ######## GUARD : FORCE ARRAY ########
  $ExistingLines = @(Get-Content -Path $Path -Encoding UTF8) | ForEach-Object { $_.TrimEnd("`r") }

  # Parse only valid rules from template
  $ParsedTemplate = Get-ParsedDefaultRules -DefaultLines $DefaultLines

  ######## SECURITY : GET FILE CONTENT WITHOUT GENERATED "NEW IGNIRE RULES" SECTION ########
  # Avoid duplication if we run script multiple times
  $CleanFileLines = Get-LinesWithoutNewRules -Lines $ExistingLines

  ######## CALCULATE : MISSING ITEMS ########
  # Ccheck if rules exist in CLEAN file content
  $CleanFileContentForCheck = $CleanFileLines | Where-Object { -not [string]::IsNullOrWhiteSpace($_) -and -not $_.StartsWith('#') }

  $ItemsToAdd = @()
  foreach ($item in $ParsedTemplate) {
    if ($CleanFileContentForCheck -notcontains $item.Rule) {
      $ItemsToAdd += $item
    }
  }

  # Build new content (clean file + new block at end)
  $NewContent = Build-GlobalGitIgnoreUpdatedContent -BaseLines $CleanFileLines -ItemsToAdd $ItemsToAdd

  # Check if content actually changed
  $CurrentContentString = $ExistingLines -join "`n"
  $NewContentString = $NewContent -join "`n"

  if ($CurrentContentString -eq $NewContentString) {
    return $false
  }

  # If we are here, we have updates
  Show-HeaderFrame -Title "UPDATE GLOBAL GIT IGNORE CONFIGURATION"

  # Display messages
  if ($ItemsToAdd.Count -gt 0) {
    $msgPrefix = "📢 New exclusions added to "
    $fileNameStr = " .gitignore_global"
    $msgSuffix = " 📢"

    $fullMsg = $msgPrefix + $fileNameStr + $msgSuffix

    Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
    Write-Host -NoNewline $msgPrefix -ForegroundColor DarkYellow
    Write-Host -NoNewline $fileNameStr -ForegroundColor Cyan
    Write-Host $msgSuffix
    Write-Host ""

    Write-Host "New rules added =>" -ForegroundColor DarkBlue

    foreach ($item in $ItemsToAdd) {
      Write-Host "  $($item.Rule)" -ForegroundColor DarkCyan
    }

    Write-Host ""
  }
  else {
    $msgPrefix = "♻️ Synchronizing "
    $fileNameStr = " .gitignore_global"
    $msgSuffix = " structure ♻️"

    $fullMsg = $msgPrefix + $fileNameStr + $msgSuffix

    Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
    Write-Host -NoNewline $msgPrefix -ForegroundColor DarkYellow
    Write-Host -NoNewline $fileNameStr -ForegroundColor Cyan
    Write-Host -NoNewline $msgSuffix -ForegroundColor DarkYellow
    Write-Host ""
  }

  ######## SECURITY : CREATE BACKUP ########
  Sync-GlobalGitIgnoreBackup -Path $Path -Action "Create"

  # Write changes
  try {
    $NewContent | Set-Content -Path $Path -Encoding UTF8 -Force

    ######## CLEANUP: DELETE BACKUP ON SUCCESS ########
    Sync-GlobalGitIgnoreBackup -Path $Path -Action "Delete"

    $msgSuccess = "✅ File updated successfully ✅"

    Write-Host -NoNewline (Get-CenteredPadding -RawMessage $msgSuccess)
    Write-Host $msgSuccess -ForegroundColor Green
    Write-Host ""

    return $true
  }
  catch {
    Show-GracefulError -Message "❌ Error updating file : " -NoCenter -ErrorDetails $_

    # Backup warning
    $msgPrefix = "⚠️ Backup saved at : "
    $msgSuffix = " $Path.bak"

    $fullMsg = $msgPrefix + $msgSuffix

    Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
    Write-Host -NoNewline $msgPrefix -ForegroundColor DarkYellow
    Write-Host $msgSuffix -ForegroundColor DarkCyan
    Write-Host ""

    return $false
  }
}

##########---------- Configure .gitignore_global reference ----------##########
function Set-GlobalGitIgnoreReference {
  param (
    [string]$Path,
    [bool]$ShowMessage
  )

  # Get actual config
  $CurrentConfig = git config --global core.excludesfile

  # Normalize paths for comparison
  $NormCurrent = if ($CurrentConfig) { $CurrentConfig.Replace('/', '\').Trim() } else { "" }
  $NormPath = $Path.Replace('/', '\').Trim()

  ######## GUARD CLAUSE : CURRENT CONFIG IS NULL OR DIFFERENT ########
  if ([string]::IsNullOrEmpty($CurrentConfig) -or ($NormCurrent -ne $NormPath)) {

    git config --global core.excludesfile $Path

    # Only show if we are in an active context (header already shown)
    if ($ShowMessage) {
      $msgPrefix = "⚓ Git configured to use "
      $msgSuffix = " .gitignore_global"

      $fullMsg = $msgPrefix + $msgSuffix

      Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
      Write-Host -NoNewline $msgPrefix -ForegroundColor DarkYellow
      Write-Host -NoNewline $msgSuffix -ForegroundColor Cyan
      Write-Host ""
    }
  }
}

##########---------- Build updated content for .gitignore_global ----------##########
function Build-GlobalGitIgnoreUpdatedContent {
  param (
    [string[]]$BaseLines,
    [array]$ItemsToAdd
  )

  # Start with clean base
  $NewContent = [System.Collections.ArrayList]@($BaseLines)

  # If no new items to add, just return cleaned file (removes old "NEW IGNORE RULES" block)
  if ($ItemsToAdd.Count -eq 0) {
    return $NewContent
  }

  # Add new rules block at end
  if ($NewContent.Count -gt 0 -and $NewContent[-1] -ne "") { $NewContent.Add("") }

  $NewContent.Add("# ======================================================================") | Out-Null
  $NewContent.Add("# NEW IGNORE RULES") | Out-Null
  $NewContent.Add("# ======================================================================") | Out-Null

  # Format and sort rules
  $FormattedBlock = Get-FormattedNewRulesBlock -Items $ItemsToAdd
  $NewContent.AddRange($FormattedBlock)

  return $NewContent
}

##########---------- Get formatted new rules block (sorting + grouping) ----------##########
function Get-FormattedNewRulesBlock {
  param (
    [array]$Items
  )

  $BlockContent = @()

  # Group missing items by category
  $GroupedRules = $Items | Group-Object Category

  foreach ($group in $GroupedRules) {
    $CategoryName = $group.Name

    # If no category (null or empty), we force "# Others"
    if ([string]::IsNullOrWhiteSpace($CategoryName)) {
      $CategoryName = "# Others"
    }
    # Otherwise, make sure it's properly formatted (double security)
    else {
      $CategoryName = Format-GitIgnoreComment -RawComment $CategoryName
    }

    # Add blank line before new category (if not immediately after header)
    if ($BlockContent.Count -gt 0) {
      $BlockContent += ""
    }

    # Add category title
    $BlockContent += $CategoryName

    # Adding rules for this group (sorted alphbetically)
    foreach ($item in ($group.Group | Sort-Object Rule)) {
      $BlockContent += $item.Rule
    }
  }

  return $BlockContent
}

##########---------- Get lines ignoring the new rules section ----------##########
function Get-LinesWithoutNewRules {
  param (
    [string[]]$Lines
  )

  $CleanLines = @()
  $Skipping = $false

  # Scan the file
  for ($i = 0; $i -lt $Lines.Length; $i++) {
    $line = $Lines[$i]

    ######## DETECTION : START OF GENERATED BLOCK ########
    # Look for decoration line followed by "NEW IGNORE RULES"
    if ($line.StartsWith("# ======================================================================") -and
      ($i + 1 -lt $Lines.Length) -and
      ($Lines[$i+1] -match "NEW IGNORE RULES")) {

      $Skipping = $true
    }

    ######## DETECTION : END OF GENERATED BLOCK ########
    if ($Skipping) {
      if ($line -match "USER CUSTOMIZATIONS") {
        # Found user customs (if accidentally placed after), stop skipping
        $Skipping = $false
      }
    }

    if (-not $Skipping) {
      $CleanLines += $line
    }
  }

  # Trim trailing empty lines from clean content
  while ($CleanLines.Count -gt 0 -and [string]::IsNullOrWhiteSpace($CleanLines[-1])) {
    $CleanLines = $CleanLines[0..($CleanLines.Count - 2)]
  }

  return $CleanLines
}

##########---------- Manage Backup (Create/Delete) ----------##########
function Sync-GlobalGitIgnoreBackup {
  param (
    [string]$Path,
    [ValidateSet("Create", "Delete")]
    [string]$Action
  )

  $BackupPath = "$Path.bak"

  if ($Action -eq "Create") {
    Copy-Item -Path $Path -Destination $BackupPath -Force
  }
  elseif ($Action -eq "Delete") {
    if (Test-Path $BackupPath) {
      Remove-Item -Path $BackupPath -Force
    }
  }
}

##########---------- Format comment (capitalize + space) ----------##########
function Format-GitIgnoreComment {
  param (
    [string]$RawComment
  )

  # If not a comment, send it back as is
  if (-not $RawComment.StartsWith("#")) { return $RawComment }

  # Remove "#"" symbol(s) and spaces from beginning
  $CleanText = $RawComment -replace "^#+\s*", ""

  # If empty after cleaning ("#"), simply return "#"
  if ([string]::IsNullOrWhiteSpace($CleanText)) { return "#" }

  # Capitalize first letter
  $FirstLetter = $CleanText.Substring(0, 1).ToUpper()
  $RestOfText = $CleanText.Substring(1)

  # Rebuilt with single guaranteed space
  return "# $FirstLetter$RestOfText"
}

##########---------- Parse template to associate rules with categories ----------##########
function Get-ParsedDefaultRules {
  param (
    [string[]]$DefaultLines
  )

  $RulesCollection = @()
  $CurrentCategory = $null

  foreach ($line in $DefaultLines) {
    # "USER CUSTOMIZATIONS" section of template isn't readable
    if ($line -match "USER CUSTOMIZATIONS") { break }

    # If line is empty, category is reset
    if ([string]::IsNullOrWhiteSpace($line)) {
      $CurrentCategory = $null

      continue
    }

    if ($line.StartsWith("#")) {
      # If comment, it's a category to include
      if ($line -match "====") {
        $CurrentCategory = $null
      }
      else {
        $CurrentCategory = Format-GitIgnoreComment -RawComment $line
      }
    }
    else {
      # If no category defined, go into "Others" category
      $RulesCollection += [PSCustomObject]@{
        Rule     = $line
        Category = $CurrentCategory
      }
    }
  }

  return $RulesCollection
}

##########---------- Provide default .gitignore_global template ----------##########
function Get-DefaultGlobalGitIgnoreTemplate {
  $Content = @'
# ======================================================================
# SECURITY & IDENTIFICATION CREDENTIALS (CRITICAL)
# ======================================================================
# Certificates & Keys
*.cert
*.key
*.pem
*.pfx
id_rsa
id_rsa.pub

# Environment Variables & Secrets
*.private.php
.env
.env.*
!.env.example
secrets.json

# ======================================================================
# SPECIFIC OS
# ======================================================================
# Linux
*~
.directory
.fuse_hidden*
.Trash-*

# macOS
._*
.AppleDouble
.DS_Store
.LSOverride
.Spotlight-V100
.Trashes

# Windows
$RECYCLE.BIN/
Desktop.ini
ehthumbs.db
Thumbs.db

# ======================================================================
# CLOUD & INFRASTRUCTURE
# ======================================================================
# Docker
*.docker.tar
.docker/
docker-compose.*.override.yml
docker-compose.override.yml

# Kubernetes / Helm
.helm/
.kube/

# Terraform
*.tfstate
*.tfstate.backup
.terraform/

# ======================================================================
# DEPLOYMENT
# ======================================================================
# Vercel
.vercel

# ======================================================================
# LANGUAGES (Compilers & Binaries)
# ======================================================================
# C# / .NET (Build Output)
[Bb]in/
[Oo]bj/
/out/
Artifacts/

# JavaScript / TypeScript (Build Output)
/bazel-out
/build
/dist
/out
/out-tsc
/tmp
*.tsbuildinfo
next-env.d.ts

# Python (Bytecode)
*.py[cod]
*$py.class
__pycache__/

# ======================================================================
# PACKAGE MANAGERS
# ======================================================================
# Composer
/vendor/

# Node.js
/node_modules
node_modules/

# Node.js (Yarn Berry)
.pnp
.pnp.*
.yarn/*
!.yarn/patches
!.yarn/plugins
!.yarn/releases
!.yarn/versions

# Npm
npm-debug.log*

# NuGet
*.nupkg
*.snupkg
**/packages/
!**/packages/build/

# PNPM
.pnpm-debug.log*

# Python (Pip / Venv)
*.egg-info/
.venv/
env/
venv/

# Typings
/typings

# Yarn
yarn-debug.log*
yarn-error.log*

# ======================================================================
# FRAMEWORKS (Cache & Configs)
# ======================================================================
# .NET Core
.store/

# Angular
.angular
/.angular/cache

# Expo / React Native
/.expo/

# Next.js
/.next/

# Sass (CSS)
.sass-cache/

# Symfony
/public/bundles/
/public/media/
/var/
/connect.lock

# ======================================================================
# TESTING (Tests & Coverage)
# ======================================================================
# General Coverage
/coverage

# Jest (JS)
.jest-cache
test-report.xml

# NUnit / MSTest (.NET)
*.trdx
*.trx
NUnitResults.xml
TestResult.xml
TestResults/

# PHPUnit (PHP)
.phpunit.result.cache
/phpunit.xml

# Testem
testem.log

# ======================================================================
# QUALITY & AUDIT
# ======================================================================
# Checkmarx
cx.*
CxReports/

# SonarQube
.scannerwork/
.sonar/
sonar-project.properties
sonar-report.json

# ======================================================================
# DOCUMENTATION
# ======================================================================
# Compodoc (Angular)
/documentation/

# Swagger / OpenAPI
api-docs/
swagger-ui/

# ======================================================================
# DATA SCIENCE & ML
# ======================================================================
# Data files
*.csv
*.parquet
/data/

# Jupyter
.ipynb_checkpoints

# MinIO / MLFlow
mc-config/
notebooks/mlruns

# ======================================================================
# IDEs & EDITORS
# ======================================================================
# Eclipse
.classpath
.project
.settings/
*.launch

# JetBrains
*.iml
*.ipr
*.iws
.idea/

# Sublime Text
*.sublime-workspace

# Visual Studio (Classic)
*.sln.docstates
*.suo
*.user
*.VisualState.xml
.vs/

# Visual Studio Code
.history/
.vscode/*
!.vscode/extensions.json
!.vscode/launch.json
!.vscode/settings.json
!.vscode/tasks.json

# ======================================================================
# DIVERS & LOGS
# ======================================================================
*.log
/libpeerconnection.log
public/COM3

# ======================================================================
# USER CUSTOMIZATIONS
# ======================================================================
/Books/Ninja Squad/
/Books/Supports Cours Formation/
'@
  # Returns an array of strings, clean CR characters
  return $Content -split "`n" | ForEach-Object { $_.TrimEnd("`r") }
}

# Executed immediately on terminal startup
Set-LoadGlobalGitIgnore
```

## Bonus

❤️ Additionally you could execute this command to copy your template in your project ❤️  
So other developpers have it to !


## Console Application Screens

![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_tuto.png)
<br><br><br>
![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_tuto_2.png)
<br><br><br>

### **File output :**  

![Output Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_output_tuto.png)

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
