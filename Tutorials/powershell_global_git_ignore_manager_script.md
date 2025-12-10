# POWERSHELL GLOBAL GIT IGNORE MANAGER SCRIPT

## SOMMAIRE

- [INTRO](#intro)
- [WHYTHISSCRIPT](#why-this-script)
- [WORKFLOW](#workflow)
- [INSTALLATION PROCEDURE](#installation-procedure)
- [BONUS](#bonus)
- [CONSOLE APPLICATION SCREENS](#console-application-screens)

## INTRO

🚀 Stop polluting my Git repositories ! 🚀  

This documentation details the architecture of the Global GitIgnore Manager, a PowerShell automation module designed to standardize, maintain, and safeguard your global git exclusion rules.  
It acts as a "Single Source of Truth" across all your development environments (Desktop, Laptops, Windows, Linux, macOS), ensuring that your git configuration remains consistent, clean, and synchronized automatically at startup.

## WHY THIS SCRIPT

😫 **The Problems =>**  

1. 🔄 **Sync & Consistency Across Machines**  
If you work on a desktop and multiple laptops, maintaining a consistent .gitignore_global is a nightmare. A rule added on one machine is often forgotten on the next.

- **Solution,** this script synchronizes your configuration. Run it once, and your environment is up to date.
- **Drift Prevention,** it ensures that your personal "Standard of Truth" is applied everywhere.

2. 🛡️ **Team Hygiene & "Dirty" Commits**  
We all have colleagues who lack rigor or use different OS/IDEs, accidentally polluting repositories with .DS_Store, Thumbs.db, or IDE configuration files.

- **Solution,** by managing global exclusions at the system level, you prevent these files from ever appearing in git status. You don't need to rely on your team remembering to ignore them => your machine ignores them by default.

3. ⚡ **Zero Friction & Offline Mode ("Set and Forget")**  
Websites like gitignore.io are great, but they require manual steps: visit site -> select tags -> generate -> copy -> open file -> paste -> save.  

- **3 Letters + Enter,** just type gir (Git Ignore Reload) in your terminal. Done.
- **No Internet Required,** works entirely offline using the embedded template.
- **Git Config Integration,** unlike text generators, this script actively configures git config --global core.excludesfile for you.

4. 🐧 **Cross-Platform Engineering**  
Built with strict adherence to .NET and PowerShell Core conventions:

- **OS Agnostic,** works seamlessly on Windows, Linux and macOS.
- **Smart Handling,** manages file encoding (UTF8/NoBOM) and path separators ("/" vs "\") automatically.

🏗 **ARCHITECTURE**  

**THE "SANDWICH" LOGIC** The script uses a hybrid Parser & Builder approach to ensure safety :

- **Standard Template :**  
Enforces the "Source of Truth" (Languages, IDEs, OS garbage files).
- **Dynamic Updates :**  
Detects the # USER CUSTOMIZATIONS section and strictly preserves everything you added manually.
- **User Customizations :**  
Injects missing rules into a dedicated section without overwriting your work.

🧠 **PHILOSOPHY**

- **Idempotency :**  
You can run the script 100 times. If nothing changed, it impacts 0ms. It only acts when necessary.
- **Non-Destructive :**  
Your private/custom rules are sacred. The script appends, it never destroys.
- **Self-Healing :**  
If you reinstall Git, crash your OS, or delete the config, the script automatically detects the anomaly and repairs the link to the global ignore file.
- **Safety First :**  
An automatic backup (.bak) is created before any changes are made. If everything went well, it is deleted.
- UX
- Cross-Platform

⚡ **TRIGGER**

- Event-Driven (startup terminal)
- CLI (alias Powershell : gir)

🛠️ **FEATURES**

1. 🧩 **Smart Merging Strategy (The Sandwich)**

- **Standard Enforcement,** ensures all rules defined in the script's template are present
- **Preservation,** automatically detects # USER CUSTOMIZATIONS section and strictly preserves everything below
- **Dynamic Injection,** injects missing rules into a dedicated # NEW IGNORE RULES block at the end of the file

2. 🧹 **Auto-Formatting & Hygiene**

- **Category Grouping,** parses comments to group rules logically
- **Alphabetical Sorting,** automatically sorts rules within their categories for better readability
- **Comment Formatting,** capitalizes and spaces comments (ex: transforms #test into # Test)
- **Clean Structure,** ensures proper spacing between sections to avoid "wall of text" files

3. 🛡️ **Crash Proof & Integrity**

- **Atomic Operation,** creates a .gitignore_global.bak copy before modifying a single line
- **Crash Recovery,** preserves the backup and displays a visual warning path if the script fails
- **Auto-Cleanup,** silently removes the backup file upon successful update

4. 🧠 **Intelligent Parsing**

- **Diff Detection,** calculates exactly which rules are missing compared to the existing file
- **Context Awareness,** avoiding duplication
- **Section Logic,** completely rebuilds the # NEW IGNORE RULES section

5. ⚓ **Git Configuration Enforcement**

- **Auto-Config,** checks and sets git config --global core.excludesfile if needed
- **Path Normalization,** handles Windows (\) vs Git (/) path separators to avoid false positives
- **Self-Healing,** automatically repairs the config if it points to a wrong or missing file

6. 📈 **Visual Feedback**

- **Status Reporting**
- **Granular Logs,** displays exactly which rules were added ( .DS_Store)
- **Error Handling,** uses shared Show-GracefulError functions for consistent error reporting

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

1. Place yourself in the directory :  

```powershell
$dir = Split-Path $PROFILE -Parent; if (!(Test-Path $dir)) { New-Item -Path $dir -ItemType Directory }; Set-Location $dir
```

2. Create the file (if it doesn't already exist) :  

```powershell
if (!(Test-Path ".\Microsoft.PowerShell_profile.ps1")) { New-Item ".\Microsoft.PowerShell_profile.ps1" -ItemType File }
```

3. Open your "Microsoft.PowerShell_profile.ps1" file in your favorite text editor.  

4. Copy/Paste "Set-LoadGlobalGitIgnore" function and his utilities functions inside.

```powershell
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

##########---------- Write file content safely (Cross-Platform encoding) ----------##########
function Set-FileContentCrossPlatform {
  param (
    [Parameter(Mandatory=$true)]
    [string]$Path,

    [Parameter(Mandatory=$true)]
    [AllowEmptyCollection()]
    [object]$Content
  )

  # Retrieves system context
  $Context = Get-SystemContext

  # By default, remain cautious (UTF8 with BOM for maximum compatibility)
  $EncodingConfig = "UTF8"

  # MODERN version of PowerShell (whether Linux, Mac or Windows 7+), uses standard without BOM
  if ($Context.IsCore) {
    $EncodingConfig = "utf8NoBOM"
  }

  # File write (uses -Value $Content rather than pipeline to ensure compatibility)
  Set-Content -Path $Path -Value $Content -Encoding $EncodingConfig -Force
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
    # Initialize content
    Set-FileContentCrossPlatform -Path $Path -Content $ContentLines

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
    # Update content
    Set-FileContentCrossPlatform -Path $Path -Content $NewContent

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

  # Cross-Platform normalization
  $NormCurrent = if (-not [string]::IsNullOrWhiteSpace($CurrentConfig)) {
    [System.IO.Path]::GetFullPath($CurrentConfig)
  }
  # Avoid .NET error
  else { "" }

  $NormPath = [System.IO.Path]::GetFullPath($Path)

  ######## GUARD CLAUSE : COMPARE STANDARDIZED VERSIONS ########
  if ($NormCurrent -ne $NormPath) {

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
# Commons
*.asc
*.cert
*.crt
*.csr
*.gpg
*.key
*.p12
*.pem
*.pfx
id_rsa
id_rsa.pub
secrets.json

# Android Studio
local.properties

# Django
local_settings.py

# Environment Variables
*.env
*.private.php
.env
.env.*

# Git
/.git-credentials
.git-credentials.local

# GitHub Actions
.env.ci
.env.github-actions
act-secrets.env
act.env

# Jenkins
*.credentials
credentials.xml
hudson.util.Secret
identity.key.enc
master.key
secrets/

# Kubernetes
*.config.yaml
*.secret.yaml
kubeconfig

# Laravel
/storage/*.key

# Local Configuration
*.local

# Terraform
*.auto.tfvars
*.tfvars.json
terraform.tfvars
terraform.tfvars.json

# ======================================================================
# SPECIFIC OS & SYSTEM FILES
# ======================================================================
# Commons
*.back.*
*.backup.*
*.bak
*.old
*.orig
*.swp
*.temp
*.tmp
*~

# Linux
.*.kate-swp
.ICEauthority
.Trash-*
.Xauthority
.bash_history
.cache/
.config/
.dbus/
.directory
.fuse_hidden*
.gvfs/
.local/
.lock
.nfs*
.recently-used
.saves-*
.swap
.thumbnails/
core
core.*
lost+found/

# macOS
.AppleDB
.AppleDB/
.AppleDesktop
.AppleDesktop/
.AppleDouble
.DS_Store
.DS_Store?
.DocumentRevisions-V100
.LSOverride
.Spotlight-V100
.TemporaryItems
.Trashes
.VolumeIcon.icns
.apdisk
.com.apple.timemachine.donotpresent
.fseventsd
._*
Icon
Network Trash Folder
Temporary Items

# Windows
$RECYCLE.BIN/
$Recycle.Bin/
*.cab
*.lnk
*.msi
*.msix
*.msm
*.msp
*.stackdump
Desktop.ini
System Volume Information/
Thumbs.db
Thumbs.db:encryptable
[Dd]esktop.ini
[Tt]emp/
ehthumbs.db
ehthumbs_vista.db
hiberfil.sys
ntuser.dat*
pagefile.sys
~$*

# ======================================================================
# FRAMEWORKS (Cache & Configs)
# ======================================================================
# .NET Core
.store/

# Android Studio
.cxx
.externalNativeBuild
.navigation/
/captures
proguard/

# Angular
.angular
/.angular/cache

# Celery
celerybeat-schedule

# Django
media/
static/

# Docusaurus
.docusaurus

# Expo & React Native
/.expo/

# Flask
.webassets-cache
instance/

# Flutter
**/android/gradlew
**/android/gradlew.bat
**/ios/Flutter/.last_build_id
.dart_tool/
.flutter-plugins
.flutter-plugins-dependencies
.metadata
.packages

# FuseBox
.fusebox/

# Laravel
/public/hot
/public/storage
storage/framework/
storage/logs/

# Next.js
.next/

# Nuxt.js
.nuxt
.output

# Parcel
.parcel-cache

# PyBuilder
.pybuilder/

# Sass (CSS)
.sass-cache/

# Scrapy
.scrapy

# Serverless
.serverless/

# SvelteKit
.svelte-kit

# Symfony
/connect.lock
/public/bundles/
/public/media/
/var/

# VuePress
.temp
.vuepress/dist

# ======================================================================
# LANGUAGES (Compilers & Specifics)
# ======================================================================
# Dart (Generated code)
*.freezed.dart
*.g.dart
*.mocks.dart

# JavaScript / TypeScript
*.tsbuildinfo
.babel.json
.cache/
.eslintcache
.rpt2_cache/
.rts2_cache_cjs/
.rts2_cache_es/
.rts2_cache_umd/
.stylelintcache
.tern-port
.v8flags.*
.webpack/
/bazel-out
/out-tsc
next-env.d.ts

# PHP
.php_cs.cache

# Python (Bytecode)
*$py.class
*.egg-info/
*.mo
*.pot
*.py[cod]
*.sage.py
.Python
.venv/
__pycache__/
cython_debug/
env/
venv/

# Python Linters & Types (Mypy, Pyre, Ruff, etc.)
.dmypy.json
.mypy_cache/
.pyre/
.pytype/
.ruff_cache/
dmypy.json
pyrightconfig.json

# ======================================================================
# IDEs & EDITORS
# ======================================================================
# Commons
.buildlog/
.idea/

# Android Studio
*.iml
.idea/.name
.idea/assetWizardSettings.xml
.idea/caches/
.idea/compiler.xml
.idea/copyright/profiles_settings.xml
.idea/encodings.xml
.idea/libraries
.idea/misc.xml
.idea/modules.xml
.idea/navEditor.xml
.idea/scopes/scope_settings.xml
.idea/tasks.xml
.idea/vcs.xml
.idea/workspace.xml

# BlueJ
*.ctxt

# Eclipse
*.launch
.apt_generated/
.classpath
.factorypath
.mtj.tmp/
.project
.settings/
.springBeans

# JetBrains (IntelliJ, WebStorm, PyCharm, etc.)
*.iml
*.iml.bak
*.ipr
*.ipr.bak
*.iws
*.iws.bak
com_crashlytics_export_strings.xml
crashlytics-build.properties
crashlytics.properties
fabric.properties

# Python IDEs (Spyder, Rope, IPython)
.ropeproject
.spyderproject
.spyproject
ipython_config.py
profile_default/

# Sublime Text
*.sublime-*
*.sublime-workspace

# Vim / Neovim
*.swl
*.swm
*.swn
*.swo
*.swp
*.un~
*.vim.old
.backup/
.netrwhist
.nvimlog
.swap/
.undo/
.vim/
.vim-bookmarks
.vim-fuf-data/
.viminfo
.vimrc.local
.vimsession
Session.vim
Sessionx.vim

# Visual Studio
*.actions
*.browsers
*.cache
*.dbmdl
*.dbproj.schemaview
*.developer.json
*.docstates
*.dotSettings.user
*.dsk
*.force.json
*.jfm
*.mbar
*.openbrowsers
*.org
*.plg
*.scope
*.sln.docstates
*.suo
*.user
*.userosscache
*.userprefs
*.usersettings
*.vcxproj.user
*.VisualState.xml
*.vsp
*.vspf
*.vspx
*.vtg
*.work
*.workspace
.vs/
_i/
_p/
_ReSharper*/
_TeamCity*

# Visual Studio Code
!.vscode/*.code-snippets
!.vscode/extensions.json
!.vscode/launch.json
!.vscode/settings.json
!.vscode/tasks.json
*.code-workspace
*.vsix
.devcontainer/
.history/
.ionide
.vscode/*
.vscode-server/
.vscode/chrome
.vscode/sftp.json
.vscode/tags
.vscodeignore

# ======================================================================
# VERSION CONTROL
# ======================================================================
# Git (Local & Temps)
*.git
.git-blame*
.git-old/
.git-rewrite/
.gitattributes.local
.gitconfig
.gitflow
.gitflow.default
.gitk
.gitlint
.gitstats
git.properties

# ======================================================================
# PACKAGE MANAGERS
# ======================================================================
# Commons
!**/packages/build/
**/packages/
.npm
bower_components/
jspm_packages/
node_modules/
typings/
vendor/
web_modules/

# Chocolatey
.choco-cache/
.chocolatey/
*.nupkg
chocolatey.config

# Composer
.composer-cache/
.composer-update-check
.composer/
composer-setup.php
composer-temp*/
composer-test-cleanup
composer.json.bak
composer.phar
composer/

# Gradle
!src/**/build/
.gradle-cache/
.gradle/
.gradletasknamecache
gradle-app.setting
gradle-build/

# Homebrew
.brew/
.homebrew-cache/
Brewfile.local
brew.env

# Maven
.m2/
.maven-cache/
.maven-classpath
.maven/
.maven_repos/
.mvn/timing.properties
buildNumber.properties
dependency-reduced-pom.xml
generated-sources/
generated-test-sources/
maven-archiver/
maven-compiler-plugin/
maven-eclipse.xml
maven-metadata-local.xml
maven-metadata.xml
maven-repository/
maven-status/
maven/
pom.xml.next
pom.xml.releaseBackup
pom.xml.tag
pom.xml.versionsBackup
release.properties
repository/

# NPM
.node-gyp/
.node_modules.ember-try/
.npm-cache/
.npm-debug/
.npm-global/
.npm-init.js
.npm-tarball/
.npm-tmp/
.npm-update-notifier
.npm-version
.npm/
.npmrc
.package-lock.json
node_modules_*
package.json.bak

# NuGet
*.nuget.cache
*.nuget.dgspec.json
*.nuget.props
*.nuget.targets
*.nupkg
*.snupkg
.nuget-packages/
.nuget-plugins/
.nuget/
.packages/
project.lock.json

# PNPM
.pnpm-cache/
.pnpm-store/
.pnpm/
pnpm-lock.yaml.bak

# Pub (Dart/Flutter)
.pub/
.pub-cache/

# Python (Pip, Pipenv, Poetry, PDM)
.eggs/
.installed.cfg
.pdm.toml
.pip-build/
.pip-cache/
.pip-tmp/
.pip/
.pip_cache/
.pipenv-cache/
.pipenv-shims/
.pipenv/
Pipfile.bak
__pypackages__/
develop-eggs/
eggs/
pip-selfcheck.json
pip-wheel-metadata/
pipenv.lock.bak
poetry.toml
pypackages/
requirements.txt.bak
requirements.txt.lock

# Yarn (v1 & Berry)
!.yarn/patches
!.yarn/plugins
!.yarn/releases
!.yarn/sdks
!.yarn/versions
.pnp
.pnp.*
.yarn-cache/
.yarn-integrity
.yarn-metadata
.yarn-metadata.json
.yarn-update-notifier
.yarn/*
.yarn/build
.yarn/build-state.yml
.yarn/cache
.yarn/install-state.gz
.yarn/install-state.gz.tmp
.yarn/unplugged
yarn-conditions.yml
yarn.build.json

# ======================================================================
# TESTING (Tests & Coverage)
# ======================================================================
# Commons
*.lcov
*.xml
.nyc_output
coverage/
lib-cov

# CMake
CTestTestfile.cmake
Testing/

# Cypress
/cypress/screenshots/
/cypress/videos/

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

# Python Testing (Coverage, Tox, Pytest, etc.)
*.cover
*.py,cover
.coverage
.coverage.*
.hypothesis/
.nox/
.pytest_cache/
.tox/
coverage.xml
cover/
htmlcov/
nosetests.xml

# VS Code (Extension Testing)
.vscode-test

# ======================================================================
# CI/CD & AUTOMATION
# ======================================================================
# CircleCI
.circle-cache/
.circleci-cache/
.circleci-local/
process.yml

# GitHub Actions
.actrc
.actions-cache/
.github-cache/
.github/runners/
.runner
.tool-versions

# GitLab CI
.gitlab-artifacts/
.gitlab-cache/
.gitlab-ci.yml.bak
.gitlab-pages/
.gitlab-runner/
.runner_system
gitlab-ci/

# Jenkins
*.groovy
*.jenkinsfile
.jenkins-workspace/
.jenkins/
builds/
init.groovy.d/
jenkins-config/
jenkins.json
jenkins.properties
jenkins.yaml
jenkins.yml
jenkins/
jenkins_backup/
jenkins_home/
plugin-configs/
plugins/
workspace/

# Travis CI
.travis-build/
.travis-cache/
.travis-data/

# Vercel
.vercel

# ======================================================================
# CLOUD & INFRASTRUCTURE
# ======================================================================
# Docker
*.docker.tar
*.env.docker
.docker-sync/
.docker/
.dockerignore
Dockerfile.*
docker-compose.*.override.yml
docker-compose.*.yaml
docker-compose.*.yml
docker-compose.dev.yml
docker-compose.local.yml
docker-compose.override.yaml
docker-compose.override.yml
docker-compose.prod.yml
docker-volumes/

# Kubernetes / Helm
*.decoded.*
.helm/
.helmignore
.kube/
kubeseal/
sealed-secrets/
values.*.yaml
values.*.yml

# Terraform
*.tfplan
*.tfstate
*.tfstate.*
*.tfstate.backup
*_override.tf
*_override.tf.json
.terraform-docs.yml
.terraform-version
.terraform.d/
.terraform.lock.hcl
.terraform/
.terragrunt-cache/
override.tf
override.tf.json
terraform.rc
terragrunt.iml

# Vagrant / Homestead
Homestead.json
Homestead.yaml

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
# Commons
site/

# Compodoc (Angular)
/documentation/

# Sphinx (Python)
docs/_build/

# Storybook
storybook-static/

# Swagger / OpenAPI
api-docs/
swagger-ui/

# ======================================================================
# DATABASES
# ======================================================================
# Commons
*.db
*.sql
*.sql.bak
*.sql~
*.sqlite

# Django
db.sqlite3
db.sqlite3-journal

# DynamoDB (Local)
.dynamodb/

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
# LOGS
# ======================================================================
# Commons
*.hprof
*.log
logs

# Specific Logs
/libpeerconnection.log
*.pid
*.pid.lock
*.seed
.node_history
.node_repl_history
.pnpm-debug.log*
action-logs/
celerybeat.pid
crash.*.log
crash.log
hs_err_pid*
jenkins_logs/
lerna-debug.log*
npm-debug.log*
pids
pip-delete-this-directory.txt
pip-log.txt
replay_pid*
report.[0-9]*.[0-9]*.[0-9]*.[0-9]*.json
runner-logs/
testem.log
yarn-debug.log*
yarn-error.log*

# Visual Studio Logs
*.svclog

# ======================================================================
# BUILDS & ARTIFACTS (Global)
# ======================================================================
# Commons
*.a
*.ap_
*.apk
*.class
*.dex
*.ear
*.egg
*.jar
*.manifest
*.nar
*.o
*.out
*.rar
*.so
*.spec
*.tar.gz
*.tgz
*.war
*.zip
.lock-wscript
[Bb]in/
[Oo]bj/
_CPack_Packages/
_build/
_deps/
Artifacts/
CMakeCache.txt
CMakeFiles/
CMakeScripts/
CMakeTmp/
MANIFEST
Makefile
build/
cmake-build-*/
cmake_install.cmake
compile_commands.json
dist/
dist-ssr/
downloads/
gen/
install_manifest.txt
lib/
lib64/
out/
parts/
sdist/
share/python-wheels/
target/
temp/
tmp/
var/
wheels/

# ======================================================================
# BUILD TOOLS & TASK RUNNERS
# ======================================================================
# CMake (Configuration)
*.cmake
CMakeCache.txt.prev
CMakeDoxyfile.in
CMakeDoxygen.in
CMakeDoxygenDefaults.cmake
CMakeLists.txt.user
CMakeLists.txt.user.*
CMakeSettings.json
CMakeUserPresets.json
DartConfiguration.tcl

# Grunt
.grunt-cache/
.grunt-tasks/
.grunt/
Gruntfile.*.js
Gruntfile.js.bak
grunt-config/
grunt.config.js

# Gulp
.gulp.*.cache
.gulp-cache/
.gulp/
gulp-cache/
gulp.1
gulp.config.js
gulpfile.*.js
gulpfile.js.bak

# Webpack
.webpack.*.cache
.webpack/
webpack.*.js
webpack.config.*.js
webpack.config.js.bak
webpack.generated.*
webpack.generated.js
webpack.records.json
webpack-assets.json
webpack-stats.json

# ======================================================================
# DESIGN & MEDIA
# ======================================================================
# Commons
*.psd
*.sketch
thumb

# ======================================================================
# MISCELLANEOUS
# ======================================================================
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

5. ⚠️ Be carefull to **ALWAYS** keep the "SHARED FUNCTIONS" section **BEFORE** the two functions stated above !!!  
On the other hand "ALIAS SECTION" could be at top...  
ℹ️ Unlike some other scripting languages ​​(such as JavaScript), PowerShell does not perform "hoisting" mechanism. You can't call a function before its declaration.

## BONUS

The main script manages your machine's hygiene. But what about your repositories?  
When you start a new project (git init), you usually need a .gitignore file immediately. Instead of copy-pasting from the web or downloading a generic file, this bonus function allows you to clone your Global Configuration into your Local Repository.  

**✨ Key Feature : Smart Privacy Filtering**  
It doesn't just copy the file blindly. It effectively sanitizes the content by stripping out the # USER CUSTOMIZATIONS section.

- **Result =>** you get a robust, standard .gitignore for your project.
- **Safety =>** your private paths (personal backup folders, specific work directories) remain on your machine and are never committed to the project.

1. Reopen your profile (see procedure above).

2. Paste the function, add this code block at the end of your file, just after the Set-LoadGlobalGitIgnore execution line.

```powershell
#-------------------------------------------------------------------------------------#
#              COPY GLOBAL GIT IGNORE CONFIG TO CURRENT REPOSITORY                    #
#-------------------------------------------------------------------------------------#

##########---------- Copy .gitignore_global to current repository ----------##########
function Copy-GlobalGitIgnoreToRepo {
  # Define Path
  $GlobalIgnorePath = Join-Path -Path $HOME -ChildPath ".gitignore_global"
  $LocalIgnorePath  = Join-Path -Path (Get-Location) -ChildPath ".gitignore"

  ######## DYNAMIC HEADER FRAME TITLE ########
  # File exists
  if (Test-Path $LocalIgnorePath) {
    $Title = "UPDATE GLOBAL GIT IGNORE IN YOUR REPOSITORY ?"
  }
  # File is missing
  else {
    $Title = "COPY GLOBAL GIT IGNORE IN YOUR REPOSITORY"
  }

  # Display header frame
  Show-HeaderFrame -Title $Title

  ######## GUARD CLAUSE : GLOBAL FILE MISSING ########
  if (-not (Test-Path $GlobalIgnorePath)) {
    Show-GracefulError -Message "⛔ .gitignore_global not found in your user folder !"
    return
  }

  # Check if repo file exists
  if (Test-Path $LocalIgnorePath) {
    $msgPrefix = " .gitignore"
    $msgSuffix = " file already exists in this repository ⚠️"

    $fullMsg = $msgPrefix + $msgSuffix

    Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
    Write-Host -NoNewline $msgPrefix -ForegroundColor Cyan
    Write-Host $msgSuffix -ForegroundColor DarkYellow
    Write-Host ""

    Write-Host -NoNewline "Overwrite it with global configuration ? (Y/n): " -ForegroundColor Magenta

    # Ask user permission
    $confirm = Wait-ForUserConfirmation

    if (-not $confirm) {
      $msgPrefix = "❌ Operation cancelled. Local "
      $fileNameStr = " .gitignore"
      $msgSuffix = " file kept."

      $fullMsg = $msgPrefix + $fileNameStr + $msgSuffix

      Write-Host ""
      Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
      Write-Host -NoNewline $msgPrefix -ForegroundColor Red
      Write-Host -NoNewline $fileNameStr -ForegroundColor Cyan
      Write-Host $msgSuffix -ForegroundColor Red
      Write-Host ""
      return
    }
  }
  # File doesn't exist
  else {
    $msgPrefix = "✨ Creating new "
    $fileNameStr = " .gitignore"
    $msgSuffix = " file from global template..."

    $fullMsg = $msgPrefix + $fileNameStr + $msgSuffix

    Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
    Write-Host -NoNewline $msgPrefix -ForegroundColor Green
    Write-Host -NoNewline $fileNameStr -ForegroundColor Cyan
    Write-Host $msgSuffix -ForegroundColor Green
    Write-Host ""
  }

  # Perform Copy
  try {
    # Read global content
    $RawContent = Get-Content -Path $GlobalIgnorePath
    $FilteredContent = New-Object System.Collections.Generic.List[string]

    foreach ($line in $RawContent) {
      ######## CASE 1 : STOP IF HIT "USER CUSTOMIZATIONS" SECTION ########
      if ($line -match "# USER CUSTOMIZATIONS") {
        # Check if line just before was a separator bar (# ========)
        $lastIndex = $FilteredContent.Count - 1
        if ($lastIndex -ge 0 -and $FilteredContent[$lastIndex] -match "^# ={5,}$") {
          $FilteredContent.RemoveAt($lastIndex)
        }

        break
      }

      ######## CASE 2 : SKIP LINES THAT ARE JUST NUMBERS ########
      if ($line -match "^\d+$") {
        continue
      }

      # Add line to new content list
      [void]$FilteredContent.Add($line)
    }

    # Set content
    Set-FileContentCrossPlatform -Path $LocalIgnorePath -Content $FilteredContent

    $msgPrefix = " .gitignore_global"
    $msgAction = " synchronized in "
    $msgTarget = " .gitignore"
    $msgSuffix = " repository ✅"

    $fullMsg = $msgPrefix + $msgAction + $msgTarget + $msgSuffix

    Write-Host ""
    Write-Host -NoNewline (Get-CenteredPadding -RawMessage $fullMsg)
    Write-Host -NoNewline $msgPrefix -ForegroundColor Cyan
    Write-Host -NoNewline $msgAction -ForegroundColor Green
    Write-Host -NoNewline $msgTarget -ForegroundColor Cyan
    Write-Host $msgSuffix -ForegroundColor Green
    Write-Host ""
  }
  catch {
    Show-GracefulError -Message "❌ Error copying file : " -NoCenter -ErrorDetails $_
  }
}
```

4. **Activate the Shortcut**  

To make it fast add this Alias definition in top of file.

```powershell
#--------------------------------------------------------------------------#
#                              ALIASES                                     #
#--------------------------------------------------------------------------#

Set-Alias gir Copy-GlobalGitIgnoreToRepo
```

5. Now you can place yourself in a local repository, open a terminal in its path and type =>  

```powershell
gir
```

**The script takes over :**  

- **If file is missing =>** It creates a perfect .gitignore derived from your global standards immediately.
- **If file exists =>** It asks for confirmation before overwriting, protecting your existing work.

The magic happens, you've just copied your .gitignore configuration with a single command.  
No more copy-pasting => **Zero friction.** 🔥🔥🔥

## Console Application Screens

![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_tuto.png)
<br><br><br>
![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_tuto_2.png)
<br><br><br>

### **File output :**  

![Output Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_output_tuto.png)

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
