# WSL
## INTRODUCTION
Windows Subsystem for Linux (WSL) est une couche de compatibilité permettant d'exécuter des exécutables binaires Linux (au format ELF) de manière native sur Windows.
## SAUVEGARDE/RESTAURATION WSL
| Commande | Description |
| :---: | :---: |
| `wsl --list --verbose` | Lister les distributions en cours d'exécution |
| `wsl --distribution <distro>` | Démarrer/Réinitialiser une distribution |
| `wsl --t <distro>` | Terminer une distribution en cours d'exécution |
| `wsl --shutdown` | Terminer toutes les distributions en cours d'exécution et le processus WSL |
| `wsl --export (distribution) (filename.tar)` | Sauvegarder une distribution WSL |
| `wsl --import (distribution) (install location) (file location and filename)` | Restaurer une distribution WSL à partir de la sauvegarde |
## LIENS SYMBOLIQUES
| Commande | Description |
| :---: | :---: |
| `sudo ln -s /mnt/c/Users/<user>/.ssh ~/.ssh` | Lier le dossier .ssh |
| `sudo ln -s /mnt/c/Users/<user>/.kube ~/.kube` | Lier le dossier .kube |
## RÉSEAU
| Commande | Description |
| :---: | :---: |
| `netsh interface portproxy add v4tov4 listenport=$port connectport=$port connectaddress=$remoteaddr` | Ajouter un transfert de port |
| `netsh advfirewall firewall add rule name=$port dir=in action=allow protocol=TCP localport=$port` | Ajouter une règle de pare-feu |
| `netsh interface portproxy delete v4tov4 listenport=$port` | Supprimer le transfert de port |
| `netsh advfirewall firewall delete rule name=$port` | Supprimer la règle de pare-feu |
| `netsh interface portproxy show v4tov4` | Afficher les transferts de port |
