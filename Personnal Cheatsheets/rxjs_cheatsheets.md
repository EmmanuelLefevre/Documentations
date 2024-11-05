# RXJS

## INTRODUCTION
RxJS (Reactive Extensions for JavaScript) est une bibliothèque JavaScript pour la programmation réactive utilisant des Observables, facilitant la composition de code asynchrone ou basé sur des événements. Elle permet de manipuler des flux de données asynchrones comme des collections, en utilisant des opérateurs puissants pour créer, transformer et combiner ces flux.  
Le système se base sur une partie Observable et une partie Souscription, l'un envoie de l'information tandis que l'autre écoute et modifie.  

[RxJS Documentation](https://rxjs.dev/)

## OPERATEURS
| Opérateurs | Fonction |
| :---: | :---: |
| `subscribe` | S'abonner à un Observable pour recevoir ses émissions. |
| `fromEvent` | Créer un Observable à partir d'un DOM event. |
| `from` | Créer un Observable à partir d'un tableau, d'un objet ou d'une promesse. |
| `pipe` | Chaîner plusieurs opérateurs et appliquer des transformations successives sur un Observable. |
| `map` | Transformer les valeurs émises par un Observable en un nouvel Observable (en appliquant une fonction). |
| `merge` | Combiner plusieurs Observables en un seul Observable (sans se soucier de l'ordre d'émission des valeurs). |
| `mergeMap` | Transformer les valeurs d'un Observable en un autre Observable et fusionner les résultats. |
| `switchMap` | Annuler les anciennes requêtes et ne garder que la dernière lorsque de nouvelles valeurs sont émises. |
| `exhaustMap` | Ignorer les nouvelles valeurs tant que l'Observable interne est actif et donc le traitement actuel non terminé. |
| `concatMap` | Transformer et concaténer les valeurs émises par un Observable de manière séquentielle. |
| `filter` | Sélectionner uniquement les valeurs d'un Observable qui satisfont une condition spécifique. |
| `every` | Vérifier si toutes les valeurs d'un Observable répondent à une condition et émettre un boolean. |
| `catchError` | Gérer les erreurs émises par un Observable. |
| `debounceTime` | Ignorer les valeurs émises par un Observable pendant une durée spécifiée. |
| `tap` | Exécuter des actions externes pour les valeurs émises par un Observable sans altérer ces valeurs. |
| `combineLatest` | Combiner plusieurs Observables pour émettre les dernières valeurs de chacun d'eux. |
| `forkJoin` | Attendre que tous les Observables terminent pour émettre les derniers résultats. |
| `take` | Limiter le nombre d’émissions d’un Observable à un certain nombre d'événements. |
| `takeUntil` | Emettre des valeurs jusqu'à ce qu'un autre Observable émette une valeur. |
| `pluck` | Extraire une propriété spécifiée d'un objet émis par un Observable. |
| `reduce` | Accumuler les valeurs d'un Observable pour produire une seule valeur finale (selon une fonction d’accumulation spécifiée). |
| `scan` | Accumuler les valeurs d'un Observable de manière incrémentielle, en émettant chaque valeur intermédiaire à chaque étape. |

## CAS D'USAGE
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `subscribe` | - Recevoir les mises à jour en temps réel d'un flux de données (comme des messages dans une application de chat). <br> - Ecouter les changements d'état d'un Observable pour mettre à jour l'interface utilisateur, par exemple en affichant un indicateur de chargement pendant une opération asynchrone. |
| `fromEvent` | - Créer un Observable qui émet des événements de clic sur un bouton pour déclencher des actions spécifiques dans l'interface utilisateur.  <br> - Ecouter les événements de saisie sur un champ de texte pour mettre à jour instantanément les données affichées à l'utilisateur. |
| `from` | - Créer un Observable à partir d'un tableau d'entiers pour émettre chaque élément en séquence. <br> - Convertir une promesse en un Observable, permettant de gérer les résultats ou les erreurs d'une opération asynchrone. |
| `pipe` | - Gestion des appels HTTP avec la manipulation des réponses. <br> - Gérer les événements de saisie d'un formulaire en temps réel pour valider et transformer les données saisies par l'utilisateur avant de les soumettre. |
| `map` | - Transformer les réponses d'une API pour ne retourner que les informations nécessaires. <br> - Extraire des propriétés spécifiques d'objets dans un tableau pour effectuer des calculs ou des affichages. |
| `merge` | - Émettre les résultats de plusieurs requêtes HTTP en parallèle et traiter les réponses lorsque toutes sont terminées. <br> - Fusionner les valeurs d'Observables émis par plusieurs capteurs dans une application IoT. |
| `mergeMap` | - Effectuer des requêtes parallèles basées sur les valeurs d'un Observable sans avoir besoin de gérer l'ordre d'éxécution. <br> - Uploader plusieurs fichiers au fil du temps (sans se soucier de l'ordre) de manière asynchrone et indépendante sans annuler l'upload précedent. |
| `switchMap` | - Lorsqu'un utilisateur modifie rapidement un champ de filtrage, switchMap peut être utilisé pour obtenir et afficher uniquement les résultats de la dernière recherche. <br> - Lors de l'abonnement à des notifications en temps réel, se désabonner des anciennes notifications lorsque l'utilisateur change de catégorie, ne recevant que celles de la catégorie actuellement sélectionnée. |
| `exhaustMap` | - Si l'utilisateur clique trop rapidement sur un bouton de suppression, les clics supplémentaires seront ignorés jusqu'à ce que la première opération de suppression soit terminée. Permet d'ignorer toutes les émissions tant que l'on a pas de retour de l'Observable. <br> - Gérer des tâches en arrière-plan qui ne peuvent pas être exécutées en parallèle. |
| `concatMap` | - Garantir que des appels HTTP indépendants sont effectués un par un dans l'ordre dans lequel ils ont été émis. <br> - Envoyer des notifications séquentiellement en fonction des événements de l'utilisateur. |
| `filter` | - Filtrer les clics sur des boutons spécifiques dans une interface utilisateur pour n'exécuter des actions que sur ceux marqués comme importants. <br> - Filtrer les données reçues d'un flux d'API pour n'afficher que les éléments qui répondent à un certain critère, comme les produits en stock dans un e-commerce. <br>  |
| `every` | - Valider que tous les items d'un panier en ligne sont disponibles avant de finaliser une commande. <br> - Vérifier si toutes les valeurs d'un flux de notes d'étudiants sont au-dessus d'une certaine moyenne pour décider de la réussite globale. |
| `catchError` | - Gérer les erreurs lors d'appels API en affichant un message d'erreur à l'utilisateur. <br> - Renvoyer une valeur par défaut lorsque la requête échoue, pour éviter de casser le flux de l'application. |
| `debounceTime` | - Attendre que l'utilisateur ait terminé de saisir dans le champ de recherche afin de limiter le nombre d'appels API. <br> - Utiliser debounceTime pour gérer les entrées d'un formulaire afin de ne valider les données qu'une fois que l'utilisateur a cessé de saisir, évitant ainsi des validations répétées. |
| `tap` | - Log les valeurs d'un Observable pour le débogage. <br> - Exécuter une fonction de notification à chaque émission. |
| `combineLatest` | - Combiner les valeurs de plusieurs champs de formulaire pour une validation dynamique. <br> - Émettre les dernières données d'une source de données combinée. |
| `forkJoin` | - Attendre que plusieurs appels API se terminent pour traiter leurs résultats ensemble. <br> - Dans une application de reporting, rassembler des informations provenant de plusieurs endpoints d'API avant de générer un rapport final. |
| `take` | - Prendre uniquement les premiers résultats d'une requête, par exemple pour une prévisualisation. <br> - Limiter les notifications d'un Observable à une seule émission, comme un message de bienvenue au premier accès d'un utilisateur. |
| `takeUntil` | - Annuler une requête de mise à jour de profil dès que l'utilisateur quitte la page. <br> - Interrompre un flux de données en direct (comme un chat) dès qu'un utilisateur se déconnecte. |
| `pluck` | - Extraire la propriété `name` d'un tableau d'objets utilisateurs. <br> - Extraire la valeur d'un input spécifique à partir d'un formulaire contenant plusieurs inputs. |
| `reduce` | - Accumuler les scores de différentes parties pour obtenir le score final dans un jeu. <br> - Calculer la somme des montants de transactions financières d'un utilisateur pour obtenir le total des dépenses. |
| `scan` | - Calculer le solde courant d'un compte après chaque transaction, en émettant le solde mis à jour à chaque opération. <br> - Suivre la progression d'un téléchargement ou d'une série d'actions en mettant à jour l'état après chaque étape réussie. |

## RESSOURCES UTILES
[Reactive How](https://reactive.how/)  

[RX Marble](https://rxmarbles.com/)  

[Think RX](https://thinkrx.io/)  

[Reactive X](https://reactivex.io/)  

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
