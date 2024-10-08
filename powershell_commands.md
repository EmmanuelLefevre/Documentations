# POWERSHELL
## Introduction
Windows PowerShell (anciennement Microsoft Command Shell (MSH) nom de code Monad) est une suite logicielle développée par Microsoft qui intègre une interface en ligne de commande, un langage de script nommé PowerShell ainsi qu'un kit de développement.
## COMMANDES UTILES



1/ Lancer en tant qu'administrateur
2/ WIN + R              ajouter cmd /k devant la commande


------IP------

afficher toutes les IP (ipv4)                           ipconfig /all 


------Cache DNS------

afficher cache DNS                                      ipconfig /displaydns >logdns.txt
effacer cache DNS                                       ipconfig /flushdns


------Shutdown (veille)------

arreter ordinateur (1h)                                 shutdown -s -t 3600
arreter ordinateur (2h)                                 shutdown -s -t 7200
annuler shutdown                                        shutdown -a
shutdown + texte                                        shutdown -s -c "text" (127 caractères maxi)
redémarrer                                              shutdown -r

------