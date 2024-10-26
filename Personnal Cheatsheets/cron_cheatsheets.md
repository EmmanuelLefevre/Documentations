# CRON
## INTRODUCTION
Cron est un programme qui permet aux utilisateurs de systèmes Unix d'exécuter automatiquement des scripts, des commandes ou des logiciels à une date et une heure prédéfinies, ou selon un cycle prédéfini.
***

| Champ | Valeurs Autorisées | Description |
| :---: | :---: | :---: |
| `SECOND` | 0-59 | Déclencher toutes les SECONDES secondes |
| `MINUTE` | 0-59 | Déclencher toutes les MINUTES minutes |
| `HOUR` | 0-23 | Déclencher toutes les HEURES heures |
| `DAY` | 1-31 | Déclencher tous les JOURS du mois |
| `MONTH` | 1-12 | Déclencher tous les MOIS |
| `DAY OF WEEK` | 0-6 | LUN-DIM Déclencher un jour spécifique de la semaine |
## CARACTÈRES SPÉCIAUX
| Caractère Spécial | Description |
| :---: | :---: |
| `*` | Déclencher à chaque unité de temps |
| `,` | Séparateur de liste |
| `–` | Spécifier une plage |
| `/` | Définir un incrément |
## EXEMPLES
| Expression Cron | Description |
| :---: | :---: |
| `0 * * * * *` | Exécuter chaque minute |
| `0 0 * * * *` | Exécuter chaque heure |
| `0 0 0 * * *` | Exécuter chaque jour |
| `0 0 0 0 * *` | Exécuter chaque mois |
| `0 0 0 1 1 *` | Exécuter le premier jour de janvier chaque année |
| `30 20 * * SAT` | Exécuter à 20h30 chaque samedi |
| `30 20 * * 6` | Exécuter à 20h30 chaque samedi |
| `0 */5 * * * *` | Exécuter toutes les cinq minutes |
| `0 0 8-10/1 * * *` | Exécuter chaque heure entre 8h et 10h |
