# RXJS

## INTRODUCTION
RxJS (Reactive Extensions for JavaScript) est une bibliothèque JavaScript pour la programmation réactive utilisant des Observables, facilitant la composition de code asynchrone ou basé sur des événements. Elle permet de manipuler des flux de données asynchrones comme des collections, en utilisant des opérateurs puissants pour créer, transformer et combiner ces flux.  
Le système se base sur une partie Observable et une partie Souscription, l'un envoie de l'information tandis que l'autre écoute et modifie.  

[RxJS Documentation](https://rxjs.dev/)

## OPERATEURS DE CREATION
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `subscribe` | S'abonner à un Observable pour recevoir ses émissions. |
| `fromEvent` | Créer un Observable à partir d'un DOM event. |
| `from` | Créer un Observable à partir d'un tableau, d'un objet ou d'une promesse. |
| `of` | Créer un Observable qui émet une série de valeurs spécifiques de façon immédiate et synchrone, et se termine aussitôt. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `subscribe` | - Recevoir les mises à jour en temps réel d'un flux de données (comme des messages dans une application de chat). <br> - Ecouter les changements d'état d'un Observable pour mettre à jour l'interface utilisateur, par exemple en affichant un indicateur de chargement pendant une opération asynchrone. |
| `fromEvent` | - Créer un Observable qui émet des événements de clic sur un bouton pour déclencher des actions spécifiques dans l'interface utilisateur.  <br> - Ecouter les événements de saisie sur un champ de texte pour mettre à jour instantanément les données affichées à l'utilisateur. |
| `from` | - Créer un Observable à partir d'un tableau d'entiers pour émettre chaque élément en séquence. <br> - Convertir une promesse en un Observable, permettant de gérer les résultats ou les erreurs d'une opération asynchrone. |
| `of` | - Simuler une séquence de valeurs (comme des valeurs constantes ou des données de test) pour simplifier les tests sans appel asynchrone. <br> - Initialiser rapidement un Observable avec une ou plusieurs valeurs précises pour un flux synchronisé. |

## OPERATEURS DE TRANSFORMATION
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `pipe` | Chaîner plusieurs opérateurs et appliquer des transformations successives sur un Observable. |
| `map` | Transformer les valeurs émises par un Observable en un nouvel Observable (en appliquant une fonction). |
| `filter` | Sélectionner uniquement les valeurs d'un Observable qui satisfont une condition spécifique. |
| `pluck` | Extraire une propriété spécifiée d'un objet émis par un Observable. |
| `tap` | Exécuter des actions externes pour les valeurs émises par un Observable sans altérer ces valeurs. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `pipe` | - Gestion des appels HTTP avec la manipulation des réponses. <br> - Gérer les événements de saisie d'un formulaire en temps réel pour valider et transformer les données saisies par l'utilisateur avant de les soumettre. |
| `map` | - Transformer les réponses d'une API pour ne retourner que les informations nécessaires. <br> - Extraire des propriétés spécifiques d'objets dans un tableau pour effectuer des calculs ou des affichages. |
| `filter` | - Filtrer les clics sur des boutons spécifiques dans une interface utilisateur pour n'exécuter des actions que sur ceux marqués comme importants. <br> - Filtrer les données reçues d'un flux d'API pour n'afficher que les éléments qui répondent à un certain critère, comme les produits en stock dans un e-commerce. <br>  |
| `pluck` | - Extraire la propriété `name` d'un tableau d'objets utilisateurs. <br> - Extraire la valeur d'un input spécifique à partir d'un formulaire contenant plusieurs inputs. |
| `tap` | - Log les valeurs d'un Observable pour le débogage. <br> - Exécuter une fonction de notification à chaque émission. |

## OPERATEURS DE COMBINAISON
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `merge` | Combiner plusieurs Observables en un seul Observable (sans se soucier de l'ordre d'émission des valeurs). |
| `mergeMap` | Transformer les valeurs d'un Observable en un autre Observable et fusionner les résultats. |
| `switchMap` | Annuler les anciennes requêtes et ne garder que la dernière lorsque de nouvelles valeurs sont émises. |
| `concatMap` | Transformer et concaténer les valeurs émises par un Observable de manière séquentielle. |
| `exhaustMap` | Ignorer les nouvelles valeurs tant que l'Observable interne est actif et donc le traitement actuel non terminé. |
| `combineLatest` | Combiner plusieurs Observables pour émettre les dernières valeurs de chacun d'eux. |
| `forkJoin` | Attendre que tous les Observables terminent pour émettre les derniers résultats. |
| `zip` | - Combiner plusieurs flux en un seul en associant les valeurs correspondantes à partir de chaque flux émis en parallèle. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `merge` | - Émettre les résultats de plusieurs requêtes HTTP en parallèle et traiter les réponses lorsque toutes sont terminées. <br> - Fusionner les valeurs d'Observables émis par plusieurs capteurs dans une application IoT. |
| `mergeMap` | - Effectuer des requêtes parallèles basées sur les valeurs d'un Observable sans avoir besoin de gérer l'ordre d'éxécution. <br> - Uploader plusieurs fichiers au fil du temps (sans se soucier de l'ordre) de manière asynchrone et indépendante sans annuler l'upload précedent. |
| `switchMap` | - Lorsqu'un utilisateur modifie rapidement un champ de filtrage, switchMap peut être utilisé pour obtenir et afficher uniquement les résultats de la dernière recherche. <br> - Lors de l'abonnement à des notifications en temps réel, se désabonner des anciennes notifications lorsque l'utilisateur change de catégorie, ne recevant que celles de la catégorie actuellement sélectionnée. |
| `concatMap` | - Garantir que des appels HTTP indépendants sont effectués un par un dans l'ordre dans lequel ils ont été émis. <br> - Envoyer des notifications séquentiellement en fonction des événements de l'utilisateur. |
| `exhaustMap` | - Si l'utilisateur clique trop rapidement sur un bouton de suppression, les clics supplémentaires seront ignorés jusqu'à ce que la première opération de suppression soit terminée. Permet d'ignorer toutes les émissions tant que l'on a pas de retour de l'Observable. <br> - Gérer des tâches en arrière-plan qui ne peuvent pas être exécutées en parallèle. |
| `combineLatest` | - Combiner les valeurs de plusieurs champs de formulaire pour une validation dynamique. <br> - Émettre les dernières données d'une source de données combinée. |
| `forkJoin` | - Attendre que plusieurs appels API se terminent pour traiter leurs résultats ensemble. <br> - Dans une application de reporting, rassembler des informations provenant de plusieurs endpoints d'API avant de générer un rapport final. |
| `zip` | - Combiner les résultats de plusieurs appels API où chaque réponse doit être traitée en parallèle, comme récupérer les informations d'un utilisateur et de ses commandes dans une application. <br> - Combiner plusieurs flux de valeurs de formulaire utilisateur pour créer un objet de données cohérent à envoyer au serveur, par exemple, combiner les flux de nom, email et mot de passe dans un formulaire d'inscription. |

## OPERATEURS DE GESTION D'ERREURS
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `catchError` | Gérer les erreurs émises par un Observable. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `catchError` | - Gérer les erreurs lors d'appels API en affichant un message d'erreur à l'utilisateur. <br> - Renvoyer une valeur par défaut lorsque la requête échoue, pour éviter de casser le flux de l'application. |

## OPERATEURS DE CONTROLE DU FLUX
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `take` | Limiter le nombre d’émissions d’un Observable à un certain nombre d'événements. |
| `takeUntil` | Emettre des valeurs jusqu'à ce qu'un autre Observable émette une valeur. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `take` | - Prendre uniquement les premiers résultats d'une requête, par exemple pour une prévisualisation. <br> - Limiter les notifications d'un Observable à une seule émission, comme un message de bienvenue au premier accès d'un utilisateur. |
| `takeUntil` | - Annuler une requête de mise à jour de profil dès que l'utilisateur quitte la page. <br> - Interrompre un flux de données en direct (comme un chat) dès qu'un utilisateur se déconnecte. |

## OPERATEURS D'AGREGATION
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `reduce` | Accumuler les valeurs d'un Observable pour produire une seule valeur finale (selon une fonction d’accumulation spécifiée). |
| `scan` | Accumuler les valeurs d'un Observable de manière incrémentielle, en émettant chaque valeur intermédiaire à chaque étape. |
| `every` | Vérifier si toutes les valeurs d'un Observable répondent à une condition et émettre un boolean. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `reduce` | - Accumuler les scores de différentes parties pour obtenir le score final dans un jeu. <br> - Calculer la somme des montants de transactions financières d'un utilisateur pour obtenir le total des dépenses. |
| `scan` | - Calculer le solde courant d'un compte après chaque transaction, en émettant le solde mis à jour à chaque opération. <br> - Suivre la progression d'un téléchargement ou d'une série d'actions en mettant à jour l'état après chaque étape réussie. |
| `every` | - Valider que tous les items d'un panier en ligne sont disponibles avant de finaliser une commande. <br> - Vérifier si toutes les valeurs d'un flux de notes d'étudiants sont au-dessus d'une certaine moyenne pour décider de la réussite globale. |

## OPERATEURS DE TEMPORISATION
Ces opérateurs permettent de gérer des comportements liés au temps, comme les retards ou la régulation de la fréquence des émissions.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `debounce` | Créer un Observable qui attend un délai après la dernière émission avant d'émettre une nouvelle valeur (réduire les émissions successives). |
| `debounceTime` | Ignorer les valeurs émises par un Observable pendant une durée spécifiée. |
| `delay` | Retarder l'émission des valeurs d'un Observable d'un certain délai. |
| `throttleTime` | Émettre une valeur à intervalle régulier en limitant la fréquence des émissions. |
| `interval` | 	Créer un Observable qui émet des valeurs à intervalles réguliers. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `debounce` | - Limiter la fréquence des émissions dans des scénarios comme la recherche en temps réel, où l'utilisateur tape rapidement, en retardant l’émission jusqu’à ce qu’il cesse de taper. <br> - Réduire les actions répétitives dans des événements fréquents comme le redimensionnement de la fenêtre, en émettant uniquement après un délai d'inactivité. |
| `debounceTime` | - Limiter les appels API dans des champs de recherche en n’envoyant un appel qu’après un délai fixe, lorsque l’utilisateur a cessé de taper. <br> - Gérer les entrées de formulaire en validant les données seulement après que l'utilisateur ait cessé de saisir pendant un délai spécifique, évitant ainsi des validations répétées. |
| `delay` | - Retarder une action dans un processus pour donner du temps avant d'effectuer une autre opération. <br> - Différer l'exécution d'un événement, par exemple, afficher une alerte après un délai spécifique. |
| `throttleTime` | - Limiter les appels API fréquents pour éviter une surcharge du serveur. <br> - Limiter la fréquence de recherche dans une barre de recherche, ne permettant de lancer la recherche que toutes les 500ms, même si l'utilisateur tape plus rapidement. |
| `interval` | - Créer un Observable pour des tâches récurrentes, comme une mise à jour d'interface à intervalle régulier. <br> - Créer un Observable qui émet un événement toutes les 10 secondes pour surveiller l'état de l'application en arrière-plan. |

## OPERATEURS DE MULTICAST
Ces opérateurs permettent de créer des flux partagés qui ne réémettent pas les mêmes valeurs pour chaque abonné.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `share` | Partager un Observable entre plusieurs abonnés. |
| `publish` | Convertir un Observable en un observable multicast en utilisant un sujet. |
| `refCount` | Connecter un Observable multicast uniquement lorsque l'un des abonnés commence à s'abonner. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `share` | - Partager une source de données entre plusieurs abonnés sans refaire les calculs ou les requêtes. <br> - Partager un stream de données entre plusieurs composants pour éviter les appels API redondants. |
| `publish` | - Diffuser un flux à plusieurs observateurs tout en maintenant un contrôle sur le début et la fin du flux. <br> - Diffuser un flux de données à plusieurs observateurs après un certain événement déclencheur, comme un clic utilisateur. |
| `refCount` | - Maintenir une connexion ouverte pendant que des abonnés sont présents et la fermer lorsque tous les abonnés se désabonnent. <br> - Maintenir une connexion WebSocket active tant qu'il y a des abonnés et la fermer dès que tous se désabonnent. |

## OPERATEURS DE MEMORISATION ET DE CACHING
Ces opérateurs sont utiles pour mémoriser les résultats des Observables afin d'éviter de recalculer ou de recharger des données à chaque émission.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `share` | Partager et mémoriser les dernières valeurs émises d'un Observable pour les nouveaux abonnés. |
| `replay` | Mémoriser les résultats d'un Observable pour éviter les appels répétitifs. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `share` | - Mémoriser les derniers résultats d'une requête API pour qu'un nouvel abonné puisse les récupérer sans faire une nouvelle requête. <br> - Partager un flux de données entre plusieurs abonnés sans déclencher plusieurs exécutions de la source, comme pour une souscription à une connexion WebSocket. |
| `replay` | - Stocker les résultats des appels d'API pour éviter des répétitions inutiles et améliorer la réactivité de l'application. <br> - Permettre à de nouveaux abonnés d'accéder aux dernières valeurs d'un flux (comme les messages d'une discussion en temps réel) même s'ils rejoignent après qu'une partie de ces messages ait été émise. |

## OPERATEURS DE GESTION DE LA CONCURRENCE
Ces opérateurs permettent de contrôler la manière dont plusieurs Observables s'exécutent en parallèle, de manière séquentielle ou contrôlée.
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `concatAll` | Transformer un Observable d'Observables en un seul Observable qui émet les résultats des Observables internes un par un. |
| `mergeAll` | Combiner les résultats de plusieurs Observables internes, émettant les valeurs dès qu'elles sont disponibles, sans attendre. |
| `switchAll` | Ne garder que le dernier Observable interne et ignorer les autres. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `concatAll` | - Exécuter une série de tâches de manière séquentielle, une à la fois. <br> - Traiter les requêtes de manière ordonnée, en attendant qu'une tâche se termine avant de commencer la suivante, comme dans un système de file d'attente. |
| `mergeAll` | - Lancer plusieurs appels API en parallèle et récupérer les résultats dès qu'ils sont prêts. <br> - Exécuter plusieurs requêtes HTTP en parallèle et traiter les réponses au fur et à mesure qu'elles arrivent, comme pour des recherches simultanées dans différents services. |
| `switchAll` | - Passer à un nouveau flux de données (comme un flux de recherche) et ignorer les précédents. <br> - Gérer un flux de navigation dans une application, où un utilisateur change fréquemment de page et seule la dernière demande doit être prise en compte. |

## OPERATEURS DE CONTROLE D'ETAT
Ces opérateurs sont utiles pour gérer des états internes en fonction des émissions de l'Observable.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `startWith` | Ajouter une valeur initiale avant la première émission de l'Observable. |
| `endWith` | Ajouter une valeur finale après la dernière émission de l'Observable. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `startWith` | - Initialiser un formulaire avec des valeurs par défaut avant que l'utilisateur n'effectue une action. <br> - Déclencher un état initial dans une application avant que les données réelles soient chargées, comme afficher un indicateur de chargement au début d'une requête. |
| `endWith` | - Ajouter une notification de fin après la fin d'un traitement dans un flux. <br> - Ajouter un message de confirmation ou une action finale après le traitement d'une série d'événements, comme un message "Opération terminée" après la soumission d'un formulaire. |

## OPERATEURS DE FLUX ET D'AUDIT
Ces opérateurs permettent d'examiner ou de traiter des valeurs d'Observables sans les altérer, souvent utilisés pour le debug. De plus ils sont souvent liés à la gestion du temps et de l'optimisation des flux d'événements.   
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `auditTime` | Emettre une valeur après un délai spécifié en attendant les émissions successives. |
| `bufferTime` | Regrouper les valeurs émises par un Observable dans des fenêtres de temps spécifiées. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `auditTime` | - Auditer les événements pendant une période de temps définie, comme les clics de souris. <br> - Mesurer le temps écoulé entre les actions d'un utilisateur sur une interface pour ajuster la réactivité d'une application, comme pour l'optimisation des animations. |
| `bufferTime` | - Regrouper des événements en lots pour les traiter en un seul paquet, comme des événements de saisie. <br> - Regrouper les messages de chat envoyés par un utilisateur en un seul envoi après une période d'inactivité, afin de réduire le nombre de requêtes serveur. |

## RESSOURCES UTILES
[Reactive How](https://reactive.how/)  

[RX Marble](https://rxmarbles.com/)  

[Think RX](https://thinkrx.io/)  

[Reactive X](https://reactivex.io/)  

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
