# TERMINALS CONFIG
## INTRODUCTION
🔥❤️ Tutorial to setup step by step awesome terminals and text editor! ❤️🔥
## IMPORTANT INFORMATIONS
🧨 If not explicitly mentioned, always place yourself in your user directory and launch your PowerShell as an admin! 🧨
## FIRST STEP
### Microsoft Store
For beginning open Microsoft Store and install PowerShell.
### PowerShell
Customize PowerShell appearance:
- Click the bottom chevron and go "parameters"
- Edit the setings.json file
- Add a color theme with your taste to personalize the prompt

[Windows Terminal Color Theme Site](https://windowsterminalthemes.dev/)
## PACKAGE MANAGERS
### Chocolatey
#### Install Chocolatey
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
#### Install Winget with Chocolatey
```shell
choco install microsoft-winget
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
NVM is used to have several versions of nodejs on the same computer and to easily change between them.
#### Install NVM
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
[NVM Personal CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/nvm_cheatsheets.md)
### NodeJs (add NPM by default)
Install NodeJs
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
Install Git
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
[Git Personal CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/git_cheatsheets.md)
### SSH
Setup a SSH connexion:  
[Personal Tutorial To Create A SSH Connexion With GPG Keys](https://github.com/EmmanuelLefevre/Documentations/blob/master/ssh_connection.md)
## FONTS
### Nerd Font
I love Fira Code Nerd Font but that’s just personal, take the one you like. The only thing is to install a nerd one, otherwise you won't have the dedicated icons.
```shell
choco install nerd-fonts-firacode
```
or Hack Nerd Font
```shell
choco install nerd-fonts-hack
```
Alternatively you can download them here without using chocolatey, you should install it manually. Just click on it, windows will do the rest 😜  
[Nerd Fonts Download Site](https://www.nerdfonts.com/font-downloads)  
Now you can use the nerd font that you downloaded in PowerShell or VsCode, just setup in each one.
## NEOVIM
Neovim is a powerfull text editor. It is directly inspired by Vim (a very common editor on Unix-type operating systems), of which it is a derivative (fork).  
[Neovim Wiki](https://github.com/neovim/neovim/blob/master/INSTALL.md)
### Install Neovim
```shell
winget install --id=Neovim.Neovim -e
```
Check installation (if the text editor opens that mean everything is good 😍)
```shell
nvim
```
Create a setting folder call "nvim":
```shell
mkdir "$env:USERPROFILE\AppData\Local\nvim"
```
Create a file named init.vim:
```shell
New-Item -Path "$env:USERPROFILE\AppData\Local\nvim\init.vim" -ItemType File
```
Copy and paste the "raw" content of this init.vim file into yours...  
[Gist BeckBrace](https://gist.github.com/BekBrace/73d1d179967a33853c5e079f68fdc93a)
### Themes
Get a theme to your liking!  
[Neovim Themes Site](https://dotfyle.com/neovim/colorscheme/trending)  
[Neovim Themes Site 2](https://vimcolorschemes.com/i/trending)  
Create a "themes" folder inside vim settings:
```shell
New-Item -Path "$env:USERPROFILE\AppData\Local\nvim\themes" -ItemType Directory
```
Now you can copy the name of your theme inside the "themes" folder.
Apply your theme inside the init.vim file:  
![Apply Neovim Theme](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/apply_neovim_theme.png)  
⚠️ Configuration isn't entirely done, don't worry if Neovim does not launch correctly! ⚠️
### Plug
**You now need to install PLUG!!!**  
[Plug Documentation](https://github.com/junegunn/vim-plug)
#### Install Plug
Run the Plug install command below in this path:  
```shell
cd "$env:USERPROFILE\AppData\Local\nvim"
```
The Plug install command:  
```shell
iwr -useb https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim |`
    ni $HOME/vimfiles/autoload/plug.vim -Force
```
Don't worry about the PowerShell alert! Select "stick anyway"...  
💎 Relaunch your init.vim, if you have nicely followed all steps Neovim should launch correctly! 💎
***
We should now install the Neovim modules thanks to Plug!  
In the lower part of Neovim you can see the prompt 🕶️  
Enter "PlugInstall" in it and validate.  
![Neovim Install Modules](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/plug_install.png)  
Wait for the installation to complete, go make yourself a coffee ☕☕☕
### Yarn
Install Yarn for Neovim
```shell
cd "$env:USERPROFILE\AppData\Local\nvim\nvim-data\plugged\coc.nvim"
```
```shell
npm install --global yarn
```
Check installation:
```shell
yarn --version
```
```shell
yarn install
```
⏳ Wait cause it's a bit long! ⏳
```shell
yarn build
```
```shell
cd "$env:USERPROFILE\AppData\Local\nvim"
```
```shell
nvim init.vim
```
![Yarn Install](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/yarn_install.png)
You have autocompletion for HTML, CSS, Javascript, Typescript in Neovim... GGWP 🤙  
[Neovim Personal CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/neovim_cheatsheets.md)
## OH-MY-POSH
[Oh-My-Posh Documentation](https://ohmyposh.dev/docs/)
Install Oh-My-Posh
### With Chocolatey
```shell
choco install oh-my-posh
```
### With Winget
```shell
winget install JanDeDobbeleer.OhMyPosh -s winget
```
Check installation:
```shell
oh-my-posh --version
```
### Make a user profile
Check if the Microsoft.PowerShell_profile.ps1 file was nicely created when installing PowerShell.
```shell
echo $PROFILE
```
***
If not:
```shell
mkdir "$env:USERPROFILE\Documents\PowerShell"
```
```shell
New-Item -Path "$env:USERPROFILE\Documents\PowerShell\Microsoft.PowerShell_profile.ps1" -ItemType File
```
***
Edit Microsoft.PowerShell_profile.ps1 with your favorite text editor and paste this inside.  
```shell
oh-my-posh init pwsh --config "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/jandedobbeleer.omp.json" | Invoke-Expression
```
### Import terminal icons
Add this in your Microsoft.PowerShell_profile.ps1:
```shell
#---------#
# MODULES #
#---------#
########## Terminal Icons ##########
Import-Module Terminal-Icons
```
**Reload your terminal**  
👨‍💻 Take a look to see what is possible with this configuration file 👨‍💻  
[PowerShell Personal Profile](https://github.com/EmmanuelLefevre/Settings/blob/main/Powershell/Microsoft.PowerShell_profile.ps1)
### Themes
Apply a prompt theme to your terminal due to Oh-My-Posh:  
[Oh-My-Posh Themes Site](https://ohmyposh.dev/docs/themes)  
For the agnoster theme for example
```shell
// Replace the other import by this one
oh-my-posh init pwsh --config "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/agnoster.omp.json" | Invoke-Expression
```
For custom ones
```shell
// Replace the other import by your custom theme
oh-my-posh init pwsh --config "$env:USERPROFILE/Documents/PowerShell/powershell_profile_custom.json" | Invoke-Expression
```
[Oh-My-Posh Personal Theme For Powershell](https://github.com/EmmanuelLefevre/Settings/blob/main/Powershell/powershell_profile_darka.json)
## GIT BASH
If they do not exist create these two files in the user directory.
```shell
New-Item -Path "$env:USERPROFILE\.bashrc" -ItemType File
```
```shell
New-Item -Path "$env:USERPROFILE\.bash_profile" -ItemType File
```
### Basic theme
File ".bashrc"
```shell
# INTERACTIVE LOGIN SHELL
## Profil loaded in Powershell
eval "$(oh-my-posh init bash --config "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/jblab_2021.omp.json")"
```
File ".bash_profile"
```shell
# NON INTERACTIVE LOGIN SHELL
## Profil loaded in VSCode
eval "$(oh-my-posh init bash --config ""https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/quick-term.omp.json"")"
```
Apply the themes:
```shell
source ~/.bashrc
```
```shell
source ~/.bash_profile
```
As you can see the themes are different between Git Bash and PowerShell!
### Custom theme
File ".bashrc"
```shell
# INTERACTIVE LOGIN SHELL
## Profil loaded in Powershell
eval "$(oh-my-posh init bash --config "$HOME\Documents\GitBash\gitbash_profile_custom.json")"
```
File ".bash_profile"
```shell
# NON INTERACTIVE LOGIN SHELL
## Profil loaded in VSCode
eval "$(oh-my-posh init bash --config "$HOME\Documents\GitBash\gitbash_profile_custom.json")"
```
Apply the themes again for effective changes.  
👨‍💻 Take a look to see what is possible with a custom theme 👨‍💻  
[Oh-My-Posh Personal Theme For Git Bash](https://github.com/EmmanuelLefevre/Settings/blob/main/GitBash/gitbash_profile_darka.json)
## Z JUMPER
The Z plugin adds z directory jumping to PowerShell, you could quickly open directories that you frequent on the command line or jump to a one without entering the exact folder name.  
[Z Jumper Documentation](https://github.com/JannesMeyer/z.ps)  
```shell
Install-Module -Name z -Force
```
## PSREADLINE
PSRReadline is a powerfull PowerShell module that gives command history, autocomplete, shortcuts and syntax highlighting.  
[PSReadLine Documentation](https://learn.microsoft.com/fr-fr/powershell/module/psreadline/?view=powershell-7.4)
### Install PSReadLine:
```shell
Install-Module -Name PSReadLine --AllowPrerelease -Scope CurrentUser -Force -SkipPublisherCheck
```
```shell
Set-PSReadLineOption -PredictionSource History
```
```shell
Set-PSReadLineOption -PredictionViewStyle ListView
```
### Import PSRReadLine in PowerShell profile
Add this in your Microsoft.PowerShell_profile.ps1:
```shell
#---------#
# MODULES #
#---------#
########## PSReadLine ##########
Import-Module PSReadLine
Set-PSReadLineKeyHandler -Key Tab -Function Complete
Set-PSReadLineOption -PredictionViewStyle ListView
```
## WSL
Upcoming section!
***
🔎🔎🔎 You could find a few of my personal settings here 🔎🔎🔎  
[Emmanuel Lefevre Personal Settings](https://github.com/EmmanuelLefevre/Settings)  
***
🔥🔥🔥 You're now a fucking swag developer!!! 🔥🔥🔥
