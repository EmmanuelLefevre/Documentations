# GITHUB DAILY PUSH AUTOMATION

## SUMMARY
- [INTRODUCTION](#introduction)
- [CONFIGURATION](#configuration)
  - [Create a repository](#create-a-repository)
  - [Create the workflow](#create-the-workflow)
  - [Setup the email repository secret](#setup-the-email-repository-secret)

## INTRODUCTION
This GitHub Actions workflow was designed to automate a daily commit, allowing the user to maintain a streak of continuous commits on GitHub without interruption. This regular process, which can avoid forgetting a daily update, contributes to the visibility of the user's activity on their GitHub profile.  
The workflow is scheduled to run automatically every day using a cron trigger under the schedule field. Alternatively, it can also be triggered manually with workflow_dispatch.  
![Run Workflow Manually](https://github.com/EmmanuelLefevre/Dotfiles/blob/main/MarkdownImg/run_workflow_manually.png)  

## CONFIGURATION
### Create a repository
Choose an explicit name like "DailyPush".  

⚠️ Set it to public visibility if you use an activity or streak tracking plugin like [GitHub Streak].  

Setup your action in your new repository =>  
DailyStreakAutomation > Settings > Actions > General  
- "Allow all actions and reusable workflows"
- "Require approval for first-time contributors"
- "Read and write permissions"  

Save each sections!

### Create the workflow
Create a workflow folder call "DailyPush":
```powershell
mkdir -p "$env:USERPROFILE\DailyPush\.github\workflows"
```
Create a file named init.vim:
```powershell
New-Item -Path "$env:USERPROFILE\DailyPush\.github\workflows\daily_push.yaml" -ItemType File
```
Copy and paste the "raw" file content of this [daily_push.yaml](https://github.com/EmmanuelLefevre/DailyPush/blob/main/.github/workflows/daily_push.yml) file into yours...  

💡 The markdown log file streak.md will be created by the workflow if it does not already exist...

### Setup the email repository secret
⚠️ Exposing your email in a YAML file (especially in a public repository) is risky!  

DailyStreakAutomation > Settings > Secrets and variables > Actions > New repository secret  

![Email Repository Secret](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/email_repository_secret.png)  

Your markdown log file 🔥  
![Streak Log Markdown](https://github.com/EmmanuelLefevre/MarkdownImg/blob/main/github_streak_log.png)  

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "[Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles)" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
