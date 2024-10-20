# TERMINALS CONFIG
## INTRODUCTION
Tutorial to setup step by step awesome terminals and text editor!
## FIRST STEPS
### Microsoft Store
Install PowerShell
### PowerShell
Customize PowerShell appearance:
- Click the bottom chevron and go "parameters"
- Edit the setings.json file
- Add a color theme with your taste to personalize the prompt  
[Windows Terminal Color Theme](https://windowsterminalthemes.dev/)
## PACKAGE MANAGERS
### Chocolatey
Install Chocolatey:  
[Chocolatey Download](https://chocolatey.org/install)
**Launch a PowerShell as admin**  
```shell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
Check installation
```shell
choco -v
```
Update it
```shell
choco upgrade chocolatey
```
### Winget
Install Winget with Chocolatey:  
[Winget Download](https://community.chocolatey.org/packages/winget-cli/1.6.2771#install)  
**Launch a PowerShell as admin**  
```shell
choco choco install microsoft-winget
```
Check installation
```shell
winget --version
```
Update it
```shell
choco upgrade microsoft-winget
```
## UTILITIES
### NVM
NVM is used to have several versions of nodejs on the same computer and to easily change them.
Install NVM:  
**Launch a PowerShell as admin**  
```shell
choco install nvm
```
Check installation
```shell
nvm --version
```
Update it
```shell
choco upgrade nvm
```
[NVM CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/nvm_cheatsheets.md)
### NodeJs (add NPM by default)
Install NodeJs:  
**Launch a PowerShell as admin**  
#### With Chocolatey
```shell
choco install nodejs
```
#### With NVM
```shell
nvm install node
```
#### With Winget
```shell
winget install -e --id OpenJs.NodeJs
```
***
Check installation
```shell
node -v
```
### Git
**Launch a PowerShell as admin**  
#### With Chocolatey
```shell
choco install git
```
#### With Winget
```shell
winget install --id Git.Git -e --source winget
```
***
Check installation
```shell
git --version
```
Update it
```shell
choco upgrade git
```
[Git CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/git_cheatsheets.md)
### SSH
Setup a SSH connexion:  
[Git CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/ssh_connection.md)
### Neovim
[Neovim CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/neovim_cheatsheets.md)
## FONTS
### Nerd Font
I love Fira Code Nerd Font but that’s just personal, take the one you like. The only thing is to install a nerd one, otherwise you won't have the dedicated icons.  
Install your nerd font with chocolatey...
```shell
choco install nerd-fonts-firacode
```
or Hack Nerd Font
```shell
choco install nerd-fonts-hack
```
Alternatively you can download them here without using chocolatey, you should install it manually (just click on it, windows will do the rest 😜):  
[Nerd Fonts Download](https://www.nerdfonts.com/font-downloads)
Now you can use the nerd font that you downloaded in PowerShell or VsCode, just setup in each one.
