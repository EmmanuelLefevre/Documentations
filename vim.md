             ---------
                VIM
             ---------


Commandes générales:
--------------------

i                                        Mode insertion
: + Entrée                               Mode commande (être en mode interactif)
Esc                                      Mode interactif
Esc puis SHIFT ZQ                        Quitter éditeur en mode insertion
:q                                       Quitter éditeur en mode commande
:w fichier.md                            Enregistrer fichier (mode commande)
:wq                                      Enregistrer et quitter
:x	                                     Enregistrer (seulement en cas de modification) et quitter
:set paste	                             Passer en mode "collage"


Commandes d'édition:
--------------------

u	                          Annuler la dernière opération
<control>-r	                  Rétablir la dernière opération annulée
.	                          Répéter la dernière opération d'édition
yy	                          Copier la ligne (4yy = 4 lignes)
dd	                          Couper la ligne (4dd = 4 lignes)
p	                          Coller après (P = insérer avant)
x	                          Effacer le caractère
dw	                          Effacer le texte jusqu'à la fin du mot
diw	                          Effacer le mot sous le curseur

TOUCHE
a	            Insertion après le curseur
A	            Insertion en fin de ligne
c	            Début d’une opération de remplacement
C	            Remplacement jusqu’en fin de ligne
i	            Insertion avant le curseur
I	            Insertion en début de ligne
o	            Insertion à la ligne suivante
O	            Insertion à la ligne précédente
R	            Active le mode écrasement
s	            Remplace un caractère
S	            Remplace une ligne entière


Commandes d'insertion:
--------------------

DEPLACEMENT DU CURSEUR
h	                Un caractère vers la gauche 
j	                Un caractère vers le bas
k	                Un caractère vers le haut
l ou ESPACE	        Un caractère vers la droite

TEXTE
w ou W	            Début du mot suivant
b ou B	            Début du mot courant
e ou E	            Fin du mot courant
)	                Début de la phrase suivante
(	                Début de la phrase précédente
}                   Début du paragraphe suivant
{                   Début du paragraphe précédent
]	                Début de la section suivante
[	                Début de la section précédente

LIGNES
^	                Début de ligne
$	                Fin de ligne
-	                Premier caractère de la ligne précédente
+ ou Entrée	        Premier caractère de la ligne suivante
H	                Première ligne de l’écran
L	                Dernière ligne de l’écran
]	                Début de la section suivante
[	                Début de la section précédente


Réglages vim:
-------------

:set numéro                              Afficher numéro des lignes