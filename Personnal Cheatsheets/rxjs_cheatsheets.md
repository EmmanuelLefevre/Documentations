# RXJS

## INTRODUCTION
RxJS (Reactive Extensions for JavaScript) est une bibliothèque JavaScript pour la programmation réactive utilisant des Observables, facilitant la composition de code asynchrone ou basé sur des événements. Elle permet de manipuler des flux de données asynchrones comme des collections, en utilisant des opérateurs puissants pour créer, transformer et combiner ces flux.

## OPERATEURS
| Opérateurs | Fonction |
| :---: | :---: |
| pipe | Chaîner plusieurs opérateurs et appliquer des transformations successives sur un Observable. |
| map |  |
| mergeMap |  |
| switchMap |  |
| exhaustMap |  |
| concatMap |  |
| filter | Sélectionner uniquement les valeurs d'un Observable qui satisfont une condition spécifique. |
| catchError |  |
| debounceTime |  |

## CAS D'USAGE
| Opérateurs | Cas d'usage |
| :---: | :--- |
| pipe | - Gestion des appels HTTP avec la manipulation des réponses. <br> - Gérer les événements de saisie d'un formulaire en temps réel pour valider et transformer les données saisies par l'utilisateur avant de les soumettre. |
| map |  |
| mergeMap |  |
| switchMap |  |
| exhaustMap |  |
| concatMap |  |
| filter | - Filtrer les clics sur des boutons spécifiques dans une interface utilisateur pour n'exécuter des actions que sur ceux marqués comme importants. <br> - Filtrer les données reçues d'un flux d'API pour n'afficher que les éléments qui répondent à un certain critère, comme les produits en stock dans un e-commerce. |
| catchError |  |
| debounceTime |  |
