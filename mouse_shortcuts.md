# MOUSE SHORTCUTS

## INTRODUCTION
Assign mouse shortcuts without builder software!  
⚠️ Your mouse must have several buttons apart from the classic ones...

## PROCEDURE
1. Download AutoHotkey:  

[AutoHotkey](https://www.autohotkey.com/)

2. Install it with the exe file...  

3. Launch it => AutoHotkey Dash program  

4. "New script" button  

5. Give a name to your script file, like "mouse_shorcuts". AutoHotkey will create the file for you, this one will have ".ahk" file extension.  

6. Copy paste this inside:
```shell
; Assigner CTRL+C au bouton B4
XButton1::Send ^c

; Assigner CTRL+V au bouton B5
XButton2::Send ^v
```
7. Double click on the file to launch the script.  

8. 😍 Bonus 😍  
Automate script at startup.  

Right click on your file > Create shortcut  

Right click on the shortcut > Cut  

Windows + R  

```shell
shell:startup
```
Paste you shortcut file inside.  

Well it's done, the script will be launch at startup 🤙

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
