# ACCESSIBILITE

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [TABLEAUX (ARRAY)](#tableaux-array)
- [OBJETS (OBJECTS)](#objets-objects)
- [DICTIONNAIRES (DICTIONARIES)](#dictionnaires-dictionaries)
- [LISTES CHAINEES (LINKED LISTS)](#listes-chainees-linked-lists)
- [PILES (STACKS)](#piles-stacks)
- [FILE (QUEUES)](#file-queues)
- [ARBRE (TREES)](#arbre-trees)
- [GRAPHS (GRAPHIQUES)](#graphs-graphiquess)
- [ENSEMBLES (SETS)](#ensembles-sets)
- [ENUMERABLE (ENUMERABLES)](#enumerable-enumerables)

## INTRODUCTION

<h3 id="a11y"><span style="color: rgb(240, 0, 212);font-size: 24px;">A11y :</span></h3>
Au-delà de l'abréviation, <span style="color: rgb(207, 120, 6);font-weight: bold;">A11y</span> est la pratique de concevoir et développer des sites web et des applications numériques pour qu'ils soient utilisables par tout le monde, y compris et surtout les personnes en situation de handicap.
<br><br>

L'objectif est de supprimer les barrières qui pourraient empêcher quelqu'un d'accéder à l'information ou d'utiliser un service. Cela inclut les handicaps (non exhaustif) :

- Visuels (cécité, basse vision, daltonisme, etc).
- Moteurs (incapacité à utiliser une souris, tremblements, etc).
- Auditifs (surdité, malentendance, etc).
- Cognitifs (troubles de l'attention, dyslexie, difficultés d'apprentissage, etc).
- Vieillesse et limitations fonctionnelles : Le vieillissement n'est pas une maladie ou un handicap en soi, mais il s'accompagne souvent de limitations fonctionnelles qui rejoignent directement les problématiques d'accessibilité. La baisse de la vue, de l'ouïe, de la dextérité motrice ou de la vitesse de traitement cognitif sont des obstacles courants. Rendre un site accessible, c'est donc aussi le rendre utilisable par une population senior grandissante.

De plus il est important de noter qu'une bonne accessibilité profite souvent à tous les utilisateurs, pas seulement à ceux en situation de handicap :

- Les sous-titres (conçus pour les sourds) aident aussi quelqu'un à regarder une vidéo dans un environnement bruyant.
- Un bon contraste de couleurs (conçu pour les malvoyants) aide tout le monde à lire un écran en plein soleil.
- Une navigation au clavier simple (conçue pour ceux qui ne peuvent utiliser une souris) peut être pratique à bien des égards (oubli de souris ou celle-ci cassée/en panne).

<span style="color: rgb(250, 3, 0);font-size: 20px;">**QUELQUES CHIFFRES** : </span>

<span style="color: rgb(240, 0, 212);font-size: 18px;font-weight: bold;">1. Sites web non accessibles en 2024</span>

<span style="color: rgb(2, 128, 245);">**Source :**</span> [The WebAIM Million](https://webaim.org/projects/million/)
- <span style="font-weight: bold;">96%</span> des sites web présentent des défaillances d'accessibilité.
- texte à faible contraste => présent sur <span style="font-weight: bold;">79%</span> des pages d'accueil.
- texte alternatif (attribut alt) manquant sur les images : <span style="font-weight: bold;">58%</span> des pages.
- liens vides : <span style="font-weight: bold;">49%</span> des pages (un lien sans texte descriptif est déroutant pour la navigation au clavier ou via un lecteur d'écran).
- étiquettes de formulaire (label) manquantes : <span style="font-weight: bold;">45%</span> des pages (sans étiquette, une personne utilisant un lecteur d'écran ne sait pas quel champ elle doit remplir).

Il est important de noter que ce test est automatisé et ne détecte qu'environ <span style="font-weight: bold;">30%</span> des problèmes potentiels. En effet il ne concerne que le "TOP 1M home page" et ne scan donc <span style="font-weight: bold;">QUE</span> la page d'accueil de ce TOP sites.
<span style="font-weight: bold;">La situation réelle est donc probablement bien pire !!!</span>

<span style="color: rgb(240, 0, 212);font-size: 18px;font-weight: bold;">2. Personnes en situation de handicap</span>
- <u>A l'échelle mondiale :</u> <span style="font-weight: bold;">1,3 milliard de personnes</span> de personnes, soit environ <span style="font-weight: bold;">16%</span> de la population mondiale (1 personne sur 6), vivent avec un handicap significatif.
<span style="color: rgb(2, 128, 245);">**Source :**</span> [OMS : rapport mondial sur la santé et le handicap 2023](https://www.who.int/fr/news-room/fact-sheets/detail/disability-and-health)
- <u>A l'échelle européenne :</u> environ <span style="font-weight: bold;">135M de personnes</span> dans l'Union Européenne, soit près d'un citoyen sur quatre (<span style="font-weight: bold;">27%</span>), déclarent une forme de handicap ou de limitation durable.
<span style="color: rgb(2, 128, 245);">**Source :**</span> [Eurostat : statistiques sur le handicap 2023/2024](https://drees.solidarites-sante.gouv.fr/sites/default/files/2025-01/Fiche%208.7%20-%20Le%20handicap%20en%20Europe.pdf)
- <u>A l'échelle de la France :</u> <span style="font-weight: bold;">12M</span> de personnes sont touchées par un handicap (1 personne sur 5).
<span style="font-weight: bold;">80%</span> des handicaps sont invisibles (troubles cognitifs, maladies chroniques invalidantes...).
<span style="color: rgb(2, 128, 245);">**Source :**</span> [Chiffres consolidés par l'INSEE et la DREES 2022-2024](https://handicap.gouv.fr/publication-drees-le-handicap-en-chiffres-edition-2024#:~:text=Les%20chiffres%20cl%C3%A9s%20du%20handicap,des%20probl%C3%A8mes%20de%20m%C3%A9moire%2C%20etc.)

<span style="color: rgb(240, 0, 212);font-size: 18px;font-weight: bold;">3. Conformité dans le secteur public français</span>
Malgré l'obligation légale (depuis la loi de 2005) pour les services publics d'être accessibles, la conformité reste très faible.

[Loi pour l'égalité des droits et des chances](https://www.info.gouv.fr/accessibilite/loi-accessibilite-cadre-legal-et-obligations#:~:text=La%20loi%20pour%20l%27%C3%A9galit%C3%A9,personnes%20en%20situation%20de%20handicap.)

Dans son baromètre de 2025, l'Observatoire révélait que le taux de conformité moyen au RGAA (Référentiel Général d'Amélioration de l'Accessibilité) pour les 250 démarches les plus utilisées par les Français stagnait en dessous de <span style="font-weight: bold;">60%</span> (loin des <span style="font-weight: bold;">100%</span> requis par la loi).

<span style="color: rgb(2, 128, 245);">**Source :**</span> [Observatoire de la qualité des démarches en ligne 2025](https://observatoire.numerique.gouv.fr/observatoire)

<h3 id="accessibilite"><span style="color: rgb(240, 0, 212);font-size: 24px;">Accessibilité :</span></h3>
L'accessibilité numérique n'est pas seulement une bonne pratique technique, c'est avant tout une obligation éthique et légale visant à construire un web inclusif.
<br><br>

Concrètement, cela signifie permettre :
- à un utilisateur malvoyant de comprendre la structure et le contenu via un lecteur d'écran. Celui-ci vocalise la page en se basant sur un balisage correct : il annonce les titres (`h1`, `h2`), décrit le rôle des éléments (`bouton`, `lien`) et lit le contenu textuel des images grâce à leur attribut `alt`.
- à un utilisateur de lecteur d'écran de bénéficier d'une prononciation correcte. En déclarant la langue principale de la page avec `<html lang="fr">`, la synthèse vocale utilise le bon accent pour tout le site.
Pour les termes anglophones, les encadrer avec un `<span>` afin de permettre au lecteur de basculer sur une prononciation anglaise juste pour ce mot, évitant ainsi une lecture confuse.
(ex : `<span lang="en">framework</span>`).
- à une personne ayant un handicap moteur (et ne pouvant utiliser de souris) de contrôler l'interface entièrement au clavier. Cela se fait principalement avec la touche `Tab` pour se déplacer entre les éléments interactifs (`liens`, `boutons`, `champs`), et les touches `Entrée` ou `Espace` pour les activer. Un balisage sémantique est essentiel car les éléments natifs comme la balise `button` ou `a` sont "focusables" par défaut.
- ou encore à un utilisateur sourd ou malentendant d'accéder au contenu d'une vidéo grâce à des `captions` (sous-titres enrichis), qui décrivent non seulement les dialogues mais aussi les sons importants (ex: `[musique entraînante]`, `[porte qui claque]`), ainsi qu'une transcription textuelle complète disponible à côté de la vidéo.

Garantir l'accessibilité, c'est s'assurer que personne n'est laissé pour compte dans l'accès à l'information et aux services.

<h3 id="juridique"><span style="color: rgb(240, 0, 212);font-size: 24px;">Cadre Légal & Juridique :</span></h3>
Le cadre légal s'appuie notamment sur la transposition en droit français des directives européennes qui visent à harmoniser les exigences en la matière.
<br><br>
<span style="font-weight: bold;">Cette directive est entrée en vigueur le 28 juin 2025 ...!!!</span>
<br><br>

[Directive Européenne sur l'Accessibilité](https://www.economie.gouv.fr/dgccrf/les-fiches-pratiques/la-nouvelle-directive-europeenne-accessibilite-pour-des-produits-et-des-services-accessibles-aux-personnes-en-situation) 

En France, le non-respect des obligations d'accessibilité numérique, définies par le RGAA, expose les organismes concernés à des sanctions financières.
Le défaut de conformité ou l'absence de publication d'une déclaration d'accessibilité peut entraîner une sanction administrative dont le montant peut s'élever jusqu'à <span style="font-weight: bold;">25K€/PAR infraction constatée</span>.

[Décret n° 2019-768 du 24 juillet 2019](https://www.legifrance.gouv.fr/jorf/id/JORFTEXT000038811937)
