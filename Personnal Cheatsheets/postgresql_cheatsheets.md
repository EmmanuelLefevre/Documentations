# POSTGRESQL
## INTRODUCTION
PostgreSQL est un système de gestion de bases de données relationnelles (SGBDR) open source connu pour sa robustesse, sa conformité aux normes SQL et ses fonctionnalités avancées.

## CONNECTION
| Commande | Description |
| :---: | :---: |
| `psql -U postgres` | Se connecter à PostgreSQL en tant qu'utilisateur `postgres` |
| `psql -U <user> -d <database>` | Se connecter à PostgreSQL en tant qu'utilisateur et base de données spécifiques |
| `psql -U <user> -d <database> -h <host>` | Se connecter à PostgreSQL sur un hôte spécifique |

## CLI
| Commande | Description |
| :---: | :---: |
| `\c <database>` | Se connecter à une base de données spécifique |
| `\password <user>` | Changer le mot de passe d'un utilisateur spécifique |
| `\l` | Lister toutes les bases de données |
| `\d+` | Afficher des informations détaillées sur divers objets de la base de données |
| `\dt` | Lister toutes les tables dans la base de données actuelle |
| `\du` | Lister tous les utilisateurs |
| `\df` | Lister toutes les fonctions |
| `\dv` | Lister toutes les vues |
| `\dn` | Lister tous les schémas |
| `\dp` | Lister toutes les permissions |
| `\di` | Lister tous les index |
| `\ds` | Lister toutes les séquences |
| `\d+` | Afficher des informations détaillées sur divers objets de la base de données |
| `\q` | Quitter psql |
| `\x` | Basculer la sortie étendue |

## BACKUP/RESTORE
| Commande | Description |
| :---: | :---: |
| `pg_dump <database> > backup.sql` | Sauvegarder une base de données dans un fichier |
| `psql <database> < backup.sql` | Restaurer une base de données à partir d'un fichier |


⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
