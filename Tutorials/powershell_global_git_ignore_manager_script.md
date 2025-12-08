# POWERSHELL GLOBAL GIT IGNORE MANAGER SCRIPT

## SOMMAIRE

- [INTRO](#intro)
- [PRESENTATION](#presentation)
- [PROCEDURE](#procedure)
- [INSTALLATIONPROCEDURE](#installation-procedure)
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

## PROCEDURE

1. **Define the template :** The Get-DefaultGlobalGitIgnoreTemplate function holds the source of truth (OS files, IDEs, Languages...).

2. **Startup Check :** On terminal launch, Set-LoadGlobalGitIgnore is called.

3. **Analysis :**

- It reads the existing .gitignore_global.
- It strips out the dynamically generated section.
- It compares the rest with the template.

4. **Actions :**

- If differences are found -> Backup -> Rebuild File -> Save -> Delete Backup.
- If identical -> Do Nothing.

## INSTALLATION PROCEDURE

1. You must open your "Microsoft.PowerShell_profile.ps1" file with your favorite text editor.

2. Copy/Paste "Set-LoadGlobalGitIgnore" function and his utilities functions inside.

```powershell

```

## Bonus

❤️ Additionally you could execute this command to copy your template in your project ❤️  
So other developpers have it to !


## Console Application Screens

![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_tuto.png)
<br><br><br>
![Script Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_tuto_2.png)
<br><br><br>
File output :
![Output Screen](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/global_git_ignore_manager_output_tuto.png)

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
