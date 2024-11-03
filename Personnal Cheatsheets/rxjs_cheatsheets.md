# RXJS

## INTRODUCTION
RxJS (Reactive Extensions for JavaScript) est une bibliothèque JavaScript pour la programmation réactive utilisant des Observables, facilitant la composition de code asynchrone ou basé sur des événements. Elle permet de manipuler des flux de données asynchrones comme des collections, en utilisant des opérateurs puissants pour créer, transformer et combiner ces flux.

## OPERATEURS
| Opérateurs | Fonction |
| :---: | :---: |
| pipe | Chaîner plusieurs opérateurs et appliquer des transformations successives sur un Observable. |
| map | Transformer les valeurs émises par un Observable en appliquant une fonction. |
| mergeMap | Transformer les valeurs d'un Observable en un autre Observable et fusionner les résultats. |
| switchMap | Annuler les anciennes requêtes et ne garder que la dernière lorsque de nouvelles valeurs sont émises. |
| exhaustMap | Ignorer les nouvelles valeurs tant que l'Observable interne est actif. |
| concatMap | Transformer et concaténer les valeurs émises par un Observable de manière séquentielle. |
| filter | Sélectionner uniquement les valeurs d'un Observable qui satisfont une condition spécifique. |
| catchError | Gérer les erreurs émises par un Observable. |
| debounceTime | Ignorer les valeurs émises par un Observable pendant une durée spécifiée. |

## CAS D'USAGE
| Opérateurs | Cas d'usage |
| :---: | :--- |
| pipe | - Gestion des appels HTTP avec la manipulation des réponses. <br> - Gérer les événements de saisie d'un formulaire en temps réel pour valider et transformer les données saisies par l'utilisateur avant de les soumettre. |
| map | - Transformer les réponses d'une API pour ne retourner que les informations nécessaires. |
| mergeMap | - Effectuer des requêtes parallèles basées sur les valeurs d'un Observable. |
| switchMap | - Gérer des requêtes de recherche pour afficher les résultats en fonction de la dernière saisie de l'utilisateur. |
| exhaustMap | - Gérer des tâches en arrière-plan qui ne peuvent pas être exécutées en parallèle. |
| concatMap | - Envoyer des notifications séquentiellement en fonction des événements de l'utilisateur. |
| filter | - Filtrer les clics sur des boutons spécifiques dans une interface utilisateur pour n'exécuter des actions que sur ceux marqués comme importants. <br> - Filtrer les données reçues d'un flux d'API pour n'afficher que les éléments qui répondent à un certain critère, comme les produits en stock dans un e-commerce. |
| catchError | - Gérer les erreurs lors d'appels API en affichant un message d'erreur à l'utilisateur. |
| debounceTime | - Limiter le nombre d'appels API pendant que l'utilisateur tape dans un champ de recherche. |
