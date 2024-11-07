# NEOVIM (fonctionne pour VIM)

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [COMMANDES UTILES](#commandes-utiles)
  - [Modes](#modes)
  - [Sauvegarder / Quitter](#sauvegarder-quitter)
  - [Commandes d'édition](#commandes-dédition)
  - [Navigation dans le texte](#Navigation-dans-le-texte)
  - [Navigation dans les lignes](#Navigation-dans-les-lignes)
  - [Déplacement du curseur](#déplacement-du-curseur)
  - [Réglages](#réglages)

## INTRODUCTION
Neovim est un éditeur de texte (logiciel permettant la manipulation de fichiers texte).  Il est directement inspiré de Vim (un éditeur très répandu sur les systèmes d’exploitation de type Unix), dont il est un dérivé (fork).

## COMMANDES UTILES
### Modes
| Command          | Description          |
| :--------------: | :------------------: |
|`i`|Mode édition|
|`:`|Mode prompt|
|`Esc`|Quitter mode édition|
|`Esc + SHIFT + ZQ`|Quitter le mode prompt|

### Sauvegarder / Quitter
| Command          | Description          |
| :--------------: | :------------------: |
|`:wq`| Sauvegarder + quitter|
|`:w`| Sauvegarder|
|`:q`| Quitter|

### Commandes d'édition
| Command          | Description          |
| :--------------: | :------------------: |
|`u`|Annuler la dernière opération|
|`<control>-r`|Rétablir la dernière opération annulée|
|`.`|Répéter la dernière opération d'édition|
|`yy`|Copier la ligne (4yy = 4 lignes)|
|`dd`|Couper la ligne (4dd = 4 lignes)|
|`p`|Coller après (P = insérer avant)|
|`x`|Effacer le caractère|
|`dw`|Effacer le texte jusqu'à la fin du mot|
|`diw`|Effacer le mot sous le curseur|
|`a`|Insertion après le curseur|
|`A`|Insertion en fin de ligne|
|`c`|Début d’une opération de remplacement|
|`C`|Remplacement jusqu’en fin de ligne|
|`i`|Insertion avant le curseur|
|`I`|Insertion en début de ligne|
|`o`|Insertion à la ligne suivante|
|`O`|Insertion à la ligne précédente|
|`R`|Active le mode écrasement|
|`s`|Remplace un caractère|
|`S`|Remplace une ligne entière|

### Navigation dans le texte
| Command          | Description          |
| :--------------: | :------------------: |
|`w` ou `W`|Début du mot suivant|
|`b` ou `B`| Début du mot courant|
|`e` ou `E`| Fin du mot courant|
|`)`|Début de la phrase suivante|
|`(`|Début de la phrase précédente|
|`}`|Début du paragraphe suivant|
|`{`|Début du paragraphe précédent|
|`]`|Début de la section suivante|
|`[`|Début de la section précédente|

### Navigation dans les lignes
| Command          | Description          |
| :--------------: | :------------------: |
|`^`|Début de ligne|
|`$`|Fin de ligne|
|`-`|Premier caractère de la ligne précédente|
|`+` ou `Entrée`|Premier caractère de la ligne suivante|
|`H`|Première ligne de l’écran|
|`L`|Dernière ligne de l’écran|
|`]`|Début de la section suivante|
|`[`|Début de la section précédente|

### Déplacement du curseur
Si flèches du clavier ne fonctionnent pas!
| Command          | Description          |
| :--------------: | :------------------: |
|`h`| Un caractère vers la gauche|
|`j`| Un caractère vers le bas|
|`k`| Un caractère vers le haut|
|`l` ou `ESPACE`| Un caractère vers la droite|

### Réglages
-------------
| Command          | Description          |
| :--------------: | :------------------: |
|`:set numéro`|Afficher numéro des lignes|

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
