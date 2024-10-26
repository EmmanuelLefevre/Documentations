# MYSQL

## INTRODUCTION
MySQL est un système de gestion de bases de données relationnelles (RDBMS). Il est distribué sous une double licence GPL et propriétaire. C'est l'un des logiciels de gestion de bases de données les plus utilisés au monde, tant par le grand public (principalement pour les applications web) que par des professionnels.

## CONNECTION
| Commande | Description |
| :---: | :---: |
| `mysql -u root -p` | Se connecter à MySQL en tant qu'utilisateur root |
| `mysql -u <user> -p` | Se connecter à MySQL en tant qu'utilisateur spécifique |
| `mysql -u root -p -h <host>` | Se connecter à MySQL sur un hôte spécifique |

## BACKUP/RESTORE
| Commande | Description |
| :---: | :---: |
| `mysqldump -u root -p <database> > backup.sql` | Sauvegarder une base de données dans un fichier |
| `mysql -u root -p <database> < backup.sql` | Restaurer une base de données à partir d'un fichier |

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
