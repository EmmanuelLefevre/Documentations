# TERMINALS CONFIG

## SUMMARY
- [INTRODUCTION](#introduction)
- [IMPORTANT INFORMATIONS](#important-informations)
- [POWERSHELL](#powershell)
    - [First step](#first-step)
    - [Package managers](#package-managers)
        - [Chocolatey](#chocolatey)
        - [Winget](#winget)
    - [Utilities](#utilities)
        - [NVM](#nvm)
        - [NodeJs (add NPM by default)](#nodejs-add-npm-by-default)
        - [Git](#git)
        - [SSH](#ssh)
    - [Fonts](#fonts)
    - [Neovim](#neovim)
    - [Plug](#plug)
    - [Yarn](#yarn)
- [OH MY POSH](#oh-my-posh)
- [GIT BASH](#git-bash)
- [Z JUMPER](#z-jumper)
- [PSREADLINE](#psreadline)
- [WSL](#wsl)

## INTRODUCTION
🔥❤️ Tutorial to setup step by step awesome terminals and text editor! ❤️🔥  

It was joinly tested on Windows 10 and 11.

## IMPORTANT INFORMATIONS
🧨 If not explicitly mentioned, always place yourself in your user directory and launch your PowerShell as an admin! 🧨

## POWERSHELL
### First step
#### Microsoft Store
For beginning open Microsoft Store and install PowerShell.
#### Customize PowerShell
Customize PowerShell appearance:
- Click the bottom chevron and go "parameters"
- Edit the setings.json file
- Add a color theme with your taste to personalize the prompt

[Windows Terminal Color Theme Site](https://windowsterminalthemes.dev/)
### Package managers
#### Chocolatey
Installation:
```powershell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
Check installation:
```shell
choco -v
```
Update it:
```shell
choco upgrade chocolatey
```
#### Winget
Installation:
```shell
choco install microsoft-winget
```
Check installation:
```shell
winget --version
```
Update it:
```shell
choco upgrade microsoft-winget
```
### Utilities
#### NVM
NVM is used to have several versions of nodejs on the same computer and to easily change between them.  
Installation:  
[NVM Download](https://github.com/coreybutler/nvm-windows)  

Click on Download.  
![Download Button](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/nvm_dwld.png)  

Click on the nvm-setup.exe asset which is the installation file for the tool.  
![File setup.exe](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/nvm_setup_exe.png)  

Run the the downloaded .exe file...  

Check installation:
```shell
nvm --version
```
Update it:
```shell
choco upgrade nvm
```
[NVM Personal CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/nvm_cheatsheets.md)
#### NodeJs (add NPM by default)
Installation:
##### With Chocolatey
```shell
choco install nodejs
```
##### With NVM
```shell
nvm install node
```
##### With Winget
```shell
winget install -e --id OpenJs.NodeJs
```
***
Check installation:
```shell
node -v
```
#### Git
Installation:
##### With Chocolatey
```shell
choco install git
```
##### With Winget
```shell
winget install --id Git.Git -e --source winget
```
***
Check installation:
```shell
git --version
```
Update it:
```shell
choco upgrade git
```
[Git Personal CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/git_cheatsheets.md)
#### SSH
Setup a SSH connexion:

[Personal Tutorial To Create A SSH Connexion With GPG Keys](https://github.com/EmmanuelLefevre/Documentations/blob/master/ssh_connection.md)
### Fonts
#### Nerd Font
I love Fira Code Nerd Font but that’s just personal, take the one you like. The only thing is to install a nerd one, otherwise you won't have the dedicated icons.
```shell
choco install nerd-fonts-firacode
```
or Hack Nerd Font
```shell
choco install nerd-fonts-hack
```
Alternatively you can download them here without using chocolatey, you should install it manually.  
Just click on it, windows will do the rest 😜

[Nerd Fonts Download Site](https://www.nerdfonts.com/font-downloads)

Now you can use the nerd font that you downloaded in PowerShell or VsCode, just setup in each one.
### Neovim
Neovim is a powerfull text editor. It is directly inspired by Vim (a very common editor on Unix-type operating systems), of which it is a derivative (fork).

[Neovim Wiki](https://github.com/neovim/neovim/blob/master/INSTALL.md)

Installation:
```shell
winget install --id=Neovim.Neovim -e
```
Check installation (if the text editor opens that mean everything is good 😍):
```shell
nvim
```
Create a setting folder call "nvim":
```powershell
mkdir "$env:USERPROFILE\AppData\Local\nvim"
```
Create a file named init.vim:
```powershell
New-Item -Path "$env:USERPROFILE\AppData\Local\nvim\init.vim" -ItemType File
```
Copy and paste the "raw" file content of this init.vim file into yours...

[Gist Beck Brace](https://gist.github.com/BekBrace/73d1d179967a33853c5e079f68fdc93a)
#### Themes
Get a theme to your liking!

[Neovim Themes Site](https://dotfyle.com/neovim/colorscheme/trending)  
[Neovim Themes Site 2](https://vimcolorschemes.com/i/trending)

Create a "themes" folder inside vim settings:
```powershell
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
```powershell
cd "$env:USERPROFILE\AppData\Local\nvim"
```
The Plug install command:  
```powershell
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
Install Yarn for Neovim:
```powershell
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
```powershell
cd "$env:USERPROFILE\AppData\Local\nvim"
```
```shell
nvim init.vim
```

![Yarn Install](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/yarn_install.png)

You have autocompletion for HTML, CSS, Javascript, Typescript in Neovim... GGWP 🤙

[Neovim Personal CheatSheets](https://github.com/EmmanuelLefevre/Documentations/blob/master/neovim_cheatsheets.md)
## OH MY POSH
[Oh-My-Posh Documentation](https://ohmyposh.dev/docs/)

Installation:
##### With Chocolatey
```shell
choco install oh-my-posh
```
##### With Winget
```shell
winget install JanDeDobbeleer.OhMyPosh -s winget
```
Check installation:
```shell
oh-my-posh --version
```
##### Make a user profile
Check if the Microsoft.PowerShell_profile.ps1 file was nicely created when installing PowerShell.
```shell
echo $PROFILE
```
***
If not:
```powershell
mkdir "$env:USERPROFILE\Documents\PowerShell"
```
```powershell
New-Item -Path "$env:USERPROFILE\Documents\PowerShell\Microsoft.PowerShell_profile.ps1" -ItemType File
```
***
Edit Microsoft.PowerShell_profile.ps1 with your favorite text editor and paste this inside.  
```powershell
oh-my-posh init pwsh --config "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/jandedobbeleer.omp.json" | Invoke-Expression
```
##### Import terminal icons
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

![PowerShell Screen](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/powershell.png)

![PowerShell Functions Screen](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/powershell2.png)

##### Themes
Apply a prompt theme to your terminal due to Oh-My-Posh:

[Oh-My-Posh Themes Site](https://ohmyposh.dev/docs/themes)

Agnoster theme for example
```powershell
// Replace the other import by this one
oh-my-posh init pwsh --config "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/agnoster.omp.json" | Invoke-Expression
```
For custom ones
```powershell
// Replace the other import by your custom theme
oh-my-posh init pwsh --config "$env:USERPROFILE/Documents/PowerShell/powershell_profile_custom.json" | Invoke-Expression
```
[Oh-My-Posh Personal Theme For Powershell](https://github.com/EmmanuelLefevre/Settings/blob/main/Powershell/powershell_profile_darka.json)
## GIT BASH
If they do not exist create these two files in the user directory.
```powershell
New-Item -Path "$env:USERPROFILE\.bashrc" -ItemType File
```
```powershell
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

![GitBash Screen](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/gitbash.png)

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
### Open your Microsoft Store
Download "Ubuntu 22.04.1 LTS" (you are free to download another distribution) and click "Open"  

⚠️ Be careful not to have any other terminals open, if this is the case close them!  

Enter an username, must be different than you windows user!!!  
Enter a password and confirm it... If you're new on Linux, you won't see passwords in the prompt, it's a security standart common to Linux distributions...  

### Update/upgrade your WSL
```shell
sudo apt-get update
```
```shell
sudo apt-get upgrade
```
### Homebrew install (optional)
Homebrew is a popular package manager, originally designed for macOS, that makes it easy to install and manage software, libraries, and other tools directly from the terminal.  

[Homebrew Documentation](https://brew.sh/)

```shell
clear
```
```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
⚠️ Don't forget the "Next steps" part... (these steps may be different depending on your version of windows and the version of Homebrew). Follow the recommendation regarding the installation of GCC.  

![Homebrew Next Steps](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/homebrew_install.png)

Upgrade Homebrew:
```shell
brew update
```
```shell
brew upgrade
```
A litlle long 💤💤💤
### Oh-My-Posh install
[Oh-My-Posh Linux Installation](https://ohmyposh.dev/docs/installation/linux)
- Homebrew installation
```shell
brew install jandedobbeleer/oh-my-posh/oh-my-posh
```
Jump directly to checking your Oh-My-Posh installation.

- Manual installation

💡 You need first to install unzip (this command only work on Ubuntu/Debian)!
```shell
sudo apt update && sudo apt install unzip -y
```
Check installation:
```shell
unzip -v
```
You could now install Oh-My-Posh
```shell
curl -s https://ohmyposh.dev/install.sh | bash -s
```
Check installation (relaunch your WSL):
```shell
oh-my-posh --version
```
### Add a nerd font
```shell
oh-my-posh font install
```
Select the font you want with your keyboard down arrow and press "Enter"
### Add a prompt theme
Create a folder in the home directory
```shell
mkdir -p ~/poshtheme && cd $_
```
```shell
curl -L -o quick-term.omp.json https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/quick-term.omp.json
```
Edit your .bashrc file and add this at end:
```shell
########## THEME ##########
# Add posh theme path
export PATH="$HOME/.local/bin:$PATH"
eval "$(oh-my-posh init bash --config '/home/<YOUR WSL USERNAME>/poshtheme/quick-term.omp.json')"
# Open WSL terminal in your user Windows folder
cd /mnt/c/Users/<YOUR WINDOWS USERNAME>
```
🔃 For apply changes, reload the configuration file 🔃
```shell
source ~/.bashrc
```
Samely as PowerShell or GitBash you could create your own theme.  

[Oh-My-Posh Personal Theme For WSL](https://github.com/EmmanuelLefevre/Settings/blob/main/WSL/wsl_profile_darka.json)

![WSL Screen](https://github.com/EmmanuelLefevre/Settings/blob/main/MarkdownImg/wsl.png)

### Disable the daily message in WSL
```shell
touch ~/.hushlogin
```

***
🔎🔎🔎 You could find a few of my personal settings here 🔎🔎🔎  

[Emmanuel Lefevre Personal Settings](https://github.com/EmmanuelLefevre/Settings)  
***
🔥🔥🔥 You're now a fucking terminal user!!! 🔥🔥🔥

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗

***

Credits:

[Beck Brace Youtube](https://www.youtube.com/watch?v=fviSilPKIhs)

[Christian Lempa Youtube](https://www.youtube.com/watch?v=AK2JE2YsKto&t=242s)
