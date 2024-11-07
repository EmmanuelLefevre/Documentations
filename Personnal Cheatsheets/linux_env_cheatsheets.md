# VARIABLES D'ENVIRONNEMENT POUR LINUX

## SOMMAIRE
- [VARIABLES D'ENVIRONNEMENT](#variables-denvironnement)
- [DEFINITION DES VARIABLES D'ENVIRONNEMENT](#definition-des-variables-denvironnement)
- [EXPANSION DES PARAMETRES](#expansion-des-parametres)

# VARIABLES D'ENVIRONNEMENT
| Variable | Description |
| :---: | :---: |
| `$HOME` | Répertoire personnel de l'utilisateur actuel |
| `$PWD` | Répertoire de travail actuel |
| `$USER` | Utilisateur actuel |
| `$SHELL` | Shell actuel |
| `$PATH` | Liste des répertoires à rechercher pour les fichiers exécutables |
| `$EDITOR` | Éditeur de texte par défaut |
| `$LANG` | Langue par défaut |
| `$HOSTNAME` | Nom d'hôte de l'ordinateur |

## DEFINITION DES VARIABLES D'ENVIRONNEMENT
| Commande | Description |
| :---: | :---: |
| `export VAR=value` | Définir une variable d'environnement |
| `export VAR=value1:value2` | Définir une variable d'environnement avec plusieurs valeurs |
| `export VAR=$VAR:value` | Ajouter une valeur à une variable d'environnement |
| `unset VAR` | Supprimer une variable d'environnement |
| `env` | Afficher toutes les variables d'environnement |
| `printenv` | Afficher toutes les variables d'environnement |
| `echo $VAR` | Afficher la valeur d'une variable d'environnement |

## EXPANSION DES PARAMETRES
| Commande | Description |
| :---: | :---: |
| `${VAR}` | Valeur de la variable `VAR` |
| `${VAR:-value}` | Valeur de `VAR` si définie, sinon `value` |
| `${VAR:=value}` | Valeur de `VAR` si définie, sinon `value` et définit `VAR` à `value` |
| `${VAR:?message}` | Valeur de `VAR` si définie, sinon imprime `message` et quitte |
| `${VAR:+value}` | Valeur de `value` si `VAR` est définie, sinon rien |

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
