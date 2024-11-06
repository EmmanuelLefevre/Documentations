# RXJS

## INTRODUCTION
RxJS (Reactive Extensions for JavaScript) est une bibliothèque JavaScript pour la programmation réactive utilisant des Observables, facilitant la composition de code asynchrone ou basé sur des événements. Elle permet de manipuler des flux de données asynchrones comme des collections, en utilisant des opérateurs puissants pour créer, transformer et combiner ces flux.  
Le système se base sur une partie Observable et une partie Souscription, l'un envoie de l'information tandis que l'autre écoute et modifie.  

[RxJS Documentation](https://rxjs.dev/)  

## SOMMAIRE
[OPERATEURS DE COMPOSITION](## OPERATEURS DE COMPOSITION)  
[OPERATEURS DE CREATION](#OPERATEURS DE CREATION)

## OPERATEURS DE COMPOSITION
Ces opérateurs sont utilisés pour combiner d'autres opérateurs dans un flux fluide, facilitant la composition, ou encore gérer des états internes en fonction des émissions de l'Observable.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `endWith` | Ajouter une valeur finale après la dernière émission de l'Observable. |
| `pipe` | Chaîner plusieurs opérateurs et appliquer des transformations successives sur un Observable. |
| `startWith` | Ajouter une valeur initiale avant la première émission de l'Observable. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `endWith` | - Ajouter une notification de fin après la fin d'un traitement dans un flux. <br> - Ajouter un message de confirmation ou une action finale après le traitement d'une série d'événements, comme un message "Opération terminée" après la soumission d'un formulaire. |
| `pipe` | - Gestion des appels HTTP avec la manipulation des réponses. <br> - Gérer les événements de saisie d'un formulaire en temps réel pour valider et transformer les données saisies par l'utilisateur avant de les soumettre. |
| `startWith` | - Initialiser un formulaire avec des valeurs par défaut avant que l'utilisateur n'effectue une action. <br> - Déclencher un état initial dans une application avant que les données réelles soient chargées, comme afficher un indicateur de chargement au début d'une requête. |

## OPERATEURS DE CREATION
Ces opérateurs servent à créer un observable à partir de différentes sources de données (tableaux, valeurs, promesses...).  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `from` | Créer un Observable à partir d'un tableau, d'un objet ou d'une promesse. |
| `fromEvent` | Créer un Observable à partir d'un DOM event. |
| `of` | Créer un Observable qui émet une série de valeurs spécifiques de façon immédiate et synchrone, et se termine aussitôt. |
| `subscribe` | S'abonner à un Observable pour recevoir ses émissions. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `from` | - Créer un Observable à partir d'un tableau d'entiers pour émettre chaque élément en séquence. <br> - Convertir une promesse en un Observable, permettant de gérer les résultats ou les erreurs d'une opération asynchrone. |
| `fromEvent` | - Créer un Observable qui émet des événements de clic sur un bouton pour déclencher des actions spécifiques dans l'interface utilisateur.  <br> - Ecouter les événements de saisie sur un champ de texte pour mettre à jour instantanément les données affichées à l'utilisateur. |
| `of` | - Simuler une séquence de valeurs (comme des valeurs constantes ou des données de test) pour simplifier les tests sans appel asynchrone. <br> - Initialiser rapidement un Observable avec une ou plusieurs valeurs précises pour un flux synchronisé. |
| `subscribe` | - S'abonner à un Observable pour recevoir ses valeurs émises et réagir en conséquence. <br> - Ecouter les changements d'état d'un Observable pour mettre à jour l'interface utilisateur, par exemple en affichant un indicateur de chargement pendant une opération asynchrone. |

## OPERATEURS DE TRANSFORMATION
Ces opérateurs modifient ou transforment les données émises par un observable.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `concatMap` | Transformer et concaténer les valeurs émises par un Observable de manière séquentielle. |
| `exhaustMap` | Ignorer les nouvelles valeurs tant que l'Observable interne est actif et donc le traitement actuel non terminé. |
| `map` | Transformer les valeurs émises par un Observable en un nouvel Observable (en appliquant une fonction). |
| `mergeAll` | Combiner les résultats de plusieurs Observables internes, émettant les valeurs dès qu'elles sont disponibles, sans attendre. |
| `pluck` | Extraire une propriété spécifiée d'un objet émis par un Observable. |
| `switch` | Annuler les émissions d'un Observable précédent dès qu'un nouvel Observable est émis et se souscrire uniquement au dernier Observable actif (basculer entre flux de données sans effectuer de transformation). |
| `switchMap` | Lorsqu'un nouvel élément est émis par un Observable source, annuler l'abonnement à l'Observable précédent et se souscrire à un nouvel Observable issu de la transformation de l'élément émis (transformer les données et de n'émettre que celles du dernier flux actif). |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `concatMap` | - Garantir que des appels HTTP indépendants sont effectués un par un dans l'ordre dans lequel ils ont été émis. <br> - Envoyer des notifications séquentiellement en fonction des événements de l'utilisateur. |
| `exhaustMap` | - Si l'utilisateur clique trop rapidement sur un bouton de suppression, les clics supplémentaires seront ignorés jusqu'à ce que la première opération de suppression soit terminée. Permet d'ignorer toutes les émissions tant que l'on a pas de retour de l'Observable. <br> - Gérer des tâches en arrière-plan qui ne peuvent pas être exécutées en parallèle. |
| `map` | - Transformer les réponses d'une API pour ne retourner que les informations nécessaires. <br> - Extraire des propriétés spécifiques d'objets dans un tableau pour effectuer des calculs ou des affichages. |
| `mergeAll` | - Lancer plusieurs appels API en parallèle et récupérer les résultats dès qu'ils sont prêts. <br> - Exécuter plusieurs requêtes HTTP en parallèle et traiter les réponses au fur et à mesure qu'elles arrivent, comme pour des recherches simultanées dans différents services. |
| `pluck` | - Extraire la propriété `name` d'un tableau d'objets utilisateurs. <br> - Extraire la valeur d'un input spécifique à partir d'un formulaire contenant plusieurs inputs. |
| `switch` | - Gérer les actions de navigation dans une interface, où le passage à une nouvelle vue annule toute émission en cours de la vue précédente. <br> - Arrêter l'écoute d'un flux de données lorsqu'un nouvel événement prend la priorité (systèmes de notification ou de mise à jour). |
| `switchMap` | - Lorsqu'un utilisateur modifie rapidement un champ de filtrage, switchMap peut être utilisé pour obtenir et afficher uniquement les résultats de la dernière recherche. <br> - Lors de l'abonnement à des notifications en temps réel, se désabonner des anciennes notifications lorsque l'utilisateur change de catégorie, ne recevant que celles de la catégorie actuellement sélectionnée. |

## OPERATEURS DE COMBINAISON
Ces opérateurs permettent de regrouper ou de combiner les flux de plusieurs observables en un seul flux.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `combineLatest` | Combiner plusieurs Observables pour émettre les dernières valeurs de chacun d'eux. |
| `concat` | Combiner plusieurs Observables de manière séquentielle, en émettant les valeurs de chaque Observable dans l'ordre. |
| `forkJoin` | Attendre que tous les Observables terminent pour émettre les derniers résultats. |
| `merge` | Combiner plusieurs Observables en un seul Observable (sans se soucier de l'ordre d'émission des valeurs). |
| `mergeMap` | Transformer les valeurs d'un Observable en un autre Observable et fusionner les résultats. |
| `zip` | - Combiner plusieurs flux en un seul en associant les valeurs correspondantes à partir de chaque flux émis en parallèle. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `combineLatest` | - Combiner les valeurs de plusieurs champs de formulaire pour une validation dynamique. <br> - Émettre les dernières données d'une source de données combinée. |
| `concat` | - Exécuter plusieurs appels API dans un ordre précis, en s'assurant que le premier est terminé avant de lancer le suivant. <br> - Combiner plusieurs flux de données (différentes pages de résultats dans une pagination). |
| `forkJoin` | - Attendre que plusieurs appels API se terminent pour traiter leurs résultats ensemble. <br> - Dans une application de reporting, rassembler des informations provenant de plusieurs endpoints d'API avant de générer un rapport final. |
| `merge` | - Émettre les résultats de plusieurs requêtes HTTP en parallèle et traiter les réponses lorsque toutes sont terminées. <br> - Fusionner les valeurs d'Observables émis par plusieurs capteurs dans une application IoT. |
| `mergeMap` | - Effectuer des requêtes parallèles basées sur les valeurs d'un Observable sans avoir besoin de gérer l'ordre d'éxécution. <br> - Uploader plusieurs fichiers au fil du temps (sans se soucier de l'ordre) de manière asynchrone et indépendante sans annuler l'upload précedent. |
| `zip` | - Combiner les résultats de plusieurs appels API où chaque réponse doit être traitée en parallèle, comme récupérer les informations d'un utilisateur et de ses commandes dans une application. <br> - Combiner plusieurs flux de valeurs de formulaire utilisateur pour créer un objet de données cohérent à envoyer au serveur (combiner les flux de nom, email et mot de passe dans un formulaire d'inscription). |

## OPERATEURS DE FILTRAGE
Ces opérateurs permettent de ne laisser passer que certaines valeurs, ou de ne prendre qu'un nombre limité de valeurs.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `distinct` | Emettre uniquement les valeurs distinctes (uniques) de l'Observable en éliminant les doublons. |
| `filter` | Sélectionner uniquement les valeurs d'un Observable qui satisfont une condition spécifique. |
| `first` | Emettre la première valeur d'un Observable qui satisfait une condition, ou la première valeur émise. |
| `last` | Emettre la dernière valeur d'un Observable qui satisfait une condition, ou la dernière valeur émise. |
| `skip` | Ignorer un certain nombre de valeurs émises par un Observable avant de commencer à les émettre. |
| `skipUntil` | Ignorer les valeurs émises jusqu'à ce qu'un autre Observable émette une valeur. |
| `skipWhile` | Ignorer les valeurs émises tant qu'une condition est vraie. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `distinct` | - Filtrer les valeurs entrantes dans un formulaire pour s'assurer que l'utilisateur ne soumet pas plusieurs fois la même donnée, comme une adresse e-mail. <br> - Afficher une liste de produits en éliminant les doublons si un même produit est ajouté plusieurs fois à partir de différentes sources de données. |
| `filter` | - Filtrer les clics sur des boutons spécifiques dans une interface utilisateur pour n'exécuter des actions que sur ceux marqués comme importants. <br> - Filtrer les données reçues d'un flux d'API pour n'afficher que les éléments qui répondent à un certain critère, comme les produits en stock dans un e-commerce. <br>  |
| `first` | - Récupérer uniquement la première réponse valable d'une API qui renvoie plusieurs résultats. <br> - Obtenir la première valeur d'un flux de données lorsque l'on souhaite exécuter une action avec la première occurrence seulement. |
| `last` | - Récupérer uniquement la dernière valeur émise par un Observable (dernière réponse d'un appel API avant de clôturer une session). <br> - Prendre la dernière valeur d'un flux de données dans un cas où seul le dernier état est nécessaire, comme la dernière mesure d'une température. |
| `skip` | - Ignorer les premiers éléments d'un flux de données (ignorer les 5 premières pages de résultats dans une pagination). <br> - Ignorer les événements ou données inutiles au début d'un flux, comme les premières erreurs dans un traitement de données. |
| `skipUntil` | - Ignorer les messages d'une application de messagerie jusqu'à ce qu'un utilisateur se connecte et soit prêt à recevoir des notifications. <br> - Ne commencer à traiter les données d'un capteur qu'une fois qu'une certaine condition (seuil de température) est atteinte. |
| `skipWhile` | - Ignorer les messages dans un chat tant qu'un utilisateur n'est pas en ligne. <br> - Filtrer les résultats d'une recherche dans une application de e-commerce en ignorant les produits hors-stock jusqu'à ce qu'une mise à jour de l'inventaire ait lieu. |

## OPERATEURS DE GESTION D'ERREURS
Ces opérateurs permettent de gérer les erreurs dans les flux, comme en effectuant une nouvelle tentative ou en capturant l'erreur.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `catchError` | Gérer les erreurs émises par un Observable. |
| `retry` | Réessayer une opération en cas d'erreur, un nombre spécifique de fois. |
| `retryWhen` | Réessayer une opération en cas d'erreur, mais avec une logique personnalisée pour chaque tentative. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `catchError` | - Gérer les erreurs lors d'appels API en affichant un message d'erreur à l'utilisateur. <br> - Renvoyer une valeur par défaut lorsque la requête échoue, pour éviter de casser le flux de l'application. |
| `retry` | - Réessayer une connexion API plusieurs fois après un échec dans le cas d'une erreur temporaire. <br> - Réessayer une requête HTTP à un serveur après une erreur de réseau. |
| `retryWhen` | - Implémenter une logique personnalisée, comme un délai exponentiel avant chaque nouvelle tentative de requête API. <br> - Réessayer une requête API uniquement si certaines conditions sont remplies (délai ou nombre spécifique de tentatives). |

## OPERATEURS D'ARRET
Ces opérateurs permettent de contrôler l'émission des éléments d'un Observable en interrompant ou en limitant la diffusion des valeurs selon certaines conditions ou critères.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `take` | Limiter le nombre de valeurs émises par un Observable à un nombre spécifié. |
| `takeUntil` | Emettre des valeurs jusqu'à ce qu'un autre Observable émette une valeur. |
| `takeWhile` | Emettre des valeurs tant qu'une condition spécifiée reste vraie. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `take` | - Limiter le nombre de résultats récupérés dans une requête API, comme prendre seulement les 5 premiers articles dans un flux de données. <br> - Limiter les notifications d'un Observable à une seule émission, comme un message de bienvenue au premier accès d'un utilisateur. |
| `takeUntil` | - Annuler une requête de mise à jour de profil dès que l'utilisateur quitte la page. <br> - Interrompre un flux de données en direct (comme un chat) dès qu'un utilisateur se déconnecte. |
| `takeWhile` | - Prendre des valeurs jusqu'à ce qu'un certain critère soit atteint, comme ne récupérer que les notes des élèves tant qu'elles sont inférieures à 15. <br> - Filtrer les événements d'une surveillance jusqu'à ce qu'une condition soit remplie, comme écouter les clics sur une page tant que l'utilisateur est sur la page d'accueil. |

## OPERATEURS D'AGREGATION
Ces opérateurs permettent de combiner ou d’accumuler les valeurs émises par un Observable, soit pour vérifier une condition globale, soit pour générer une valeur cumulée ou progressive à partir de l'ensemble des émissions.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `every` | Vérifier si toutes les valeurs d'un Observable répondent à une condition et émettre un boolean. |
| `groupBy` | Permet de regrouper les éléments d'une collection (ou flux de données) en sous-groupes, en fonction d'une clé spécifique extraite de chaque élément. |
| `reduce` | Accumuler les valeurs d'un Observable pour produire une seule valeur finale (selon une fonction d’accumulation spécifiée). |
| `scan` | Accumuler les valeurs d'un Observable de manière incrémentielle, en émettant chaque valeur intermédiaire à chaque étape. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `every` | - Valider que tous les items d'un panier en ligne sont disponibles avant de finaliser une commande. <br> - Vérifier si toutes les valeurs d'un flux de notes d'étudiants sont au-dessus d'une certaine moyenne pour décider de la réussite globale. |
| `groupBy` | - Regrouper des utilisateurs par âge dans une application de gestion de profils pour analyser les comportements par tranche d'âge. <br> - Regrouper les ventes par produit dans une application de suivi des transactions pour analyser la performance de chaque article. |
| `reduce` | - Accumuler les scores de différentes parties pour obtenir le score final dans un jeu. <br> - Calculer la somme des montants de transactions financières d'un utilisateur pour obtenir le total des dépenses. |
| `scan` | - Calculer le solde courant d'un compte après chaque transaction, en émettant le solde mis à jour à chaque opération. <br> - Suivre la progression d'un téléchargement ou d'une série d'actions en mettant à jour l'état après chaque étape réussie. |

## OPERATEURS DE SOUSCRIPTION
Ces opérateurs sont conçus pour contrôler les souscriptions à des Observables en garantissant qu'une nouvelle émission ne déclenchera pas une nouvelle souscription tant que celle en cours n'est pas terminée.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `exhaust` | Ignorer les émissions tant qu'un Observable précédent est toujours en cours. |
| `race` |  |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `exhaust` | - Dans une IHM si un utilisateur clique trop rapidement sur un bouton de soumission, exhaust ignorera tous les clics supplémentaires tant que la première action n'est pas terminée. <br> - Lorsqu'un utilisateur lance plusieurs téléchargements en parallèle, exhaust peut être utilisé pour s'assurer qu'un nouveau téléchargement ne commencera pas tant que le téléchargement en cours n'est pas terminé. |
| `race` |  |

## OPERATEURS DE TEMPORISATION
Ces opérateurs permettent de gérer des comportements liés au temps, comme les retards ou la régulation de la fréquence des émissions.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `debounce` | Créer un Observable qui attend un délai après la dernière émission avant d'émettre une nouvelle valeur (réduire les émissions successives). |
| `debounceTime` | Ignorer les valeurs émises par un Observable pendant une durée spécifiée. |
| `delay` | Retarder l'émission des valeurs d'un Observable d'un certain délai. |
| `interval` | 	Créer un Observable qui émet des valeurs à intervalles réguliers. |
| `throttleTime` | Émettre une valeur à intervalle régulier en limitant la fréquence des émissions. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `debounce` | - Limiter la fréquence des émissions dans des scénarios comme la recherche en temps réel, où l'utilisateur tape rapidement, en retardant l’émission jusqu’à ce qu’il cesse de taper. <br> - Réduire les actions répétitives dans des événements fréquents comme le redimensionnement de la fenêtre, en émettant uniquement après un délai d'inactivité. |
| `debounceTime` | - Limiter les appels API dans des champs de recherche en n’envoyant un appel qu’après un délai fixe, lorsque l’utilisateur a cessé de taper. <br> - Gérer les entrées de formulaire en validant les données seulement après que l'utilisateur ait cessé de saisir pendant un délai spécifique, évitant ainsi des validations répétées. |
| `delay` | - Retarder une action dans un processus pour donner du temps avant d'effectuer une autre opération. <br> - Différer l'exécution d'un événement (afficher une alerte après un délai spécifique). |
| `interval` | - Créer un Observable pour des tâches récurrentes, comme une mise à jour d'interface à intervalle régulier. <br> - Créer un Observable qui émet un événement toutes les 10 secondes pour surveiller l'état de l'application en arrière-plan. |
| `throttleTime` | - Limiter les appels API fréquents pour éviter une surcharge du serveur. <br> - Limiter la fréquence de recherche dans une barre de recherche, ne permettant de lancer la recherche que toutes les 500ms, même si l'utilisateur tape plus rapidement. |

## OPERATEURS DE MULTICAST
Ces opérateurs permettent de créer des flux partagés qui ne réémettent pas les mêmes valeurs pour chaque abonné.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `publish` | Convertir un Observable en un observable multicast en utilisant un sujet. |
| `refCount` | Connecter un Observable multicast uniquement lorsque l'un des abonnés commence à s'abonner. |
| `share` | Partager et mémoriser les dernières valeurs émises d'un Observable pour les nouveaux abonnés. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `publish` | - Diffuser un flux à plusieurs observateurs tout en maintenant un contrôle sur le début et la fin du flux. <br> - Diffuser un flux de données à plusieurs observateurs après un certain événement déclencheur, comme un clic utilisateur. |
| `refCount` | - Maintenir une connexion ouverte pendant que des abonnés sont présents et la fermer lorsque tous les abonnés se désabonnent. <br> - Maintenir une connexion WebSocket active tant qu'il y a des abonnés et la fermer dès que tous se désabonnent. |
| `share` | - Mémoriser les derniers résultats d'une requête API pour qu'un nouvel abonné puisse les récupérer sans faire une nouvelle requête. <br> - Partager un flux de données entre plusieurs abonnés sans déclencher plusieurs exécutions de la source, comme pour une souscription à une connexion WebSocket. |

## OPERATEUR DE MEMORISATION ET DE CACHING
Ces opérateurs sont utiles pour mémoriser les résultats des Observables afin d'éviter de recalculer ou de recharger des données à chaque émission.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `replay` | Mémoriser les résultats d'un Observable pour éviter les appels répétitifs. |
| `shareReplay` |  |
| `publishReplay` |  |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `replay` | - Stocker les résultats des appels d'API pour éviter des répétitions inutiles et améliorer la réactivité de l'application. <br> - Permettre à de nouveaux abonnés d'accéder aux dernières valeurs d'un flux (comme les messages d'une discussion en temps réel) même s'ils rejoignent après qu'une partie de ces messages ait été émise. |
| `shareReplay` |  |
| `publishReplay` |  |

## OPERATEURS DE GESTION DE LA CONCURRENCE
Ces opérateurs permettent de contrôler la manière dont plusieurs Observables s'exécutent en parallèle, de manière séquentielle ou contrôlée.
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `concatAll` | Transformer un Observable d'Observables en un seul Observable qui émet les résultats des Observables internes un par un. |
| `switchAll` | Ne garder que le dernier Observable interne et ignorer les autres. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `concatAll` | - Exécuter une série de tâches de manière séquentielle, une à la fois. <br> - Traiter les requêtes de manière ordonnée, en attendant qu'une tâche se termine avant de commencer la suivante, comme dans un système de file d'attente. |
| `switchAll` | - Passer à un nouveau flux de données (comme un flux de recherche) et ignorer les précédents. <br> - Gérer un flux de navigation dans une application, où un utilisateur change fréquemment de page et seule la dernière demande doit être prise en compte. |

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

## OPERATEURS UTILITAIRES
Ces opérateurs sont principalement utilisés pour des tâches annexes comme le débogage, l’ajout de délais, ou la gestion de l'exécution en arrière-plan.  
### **Opérateurs**
| Opérateurs | Fonction |
| :---: | :---: |
| `delay` | Retarder l'émission des valeurs d'un Observable de la durée spécifiée. |
| `finalize` | Exécuter une action lorsqu'un Observable termine son exécution, qu'il émet une valeur ou qu'il génère une erreur. |
| `tap` | Exécuter des actions externes pour les valeurs émises par un Observable sans altérer ces valeurs. |
| `timeout` | Emettre une erreur si l'Observable ne renvoie pas de valeur dans un délai spécifié. |

### **Cas d'usage**
| Opérateurs | Cas d'usage |
| :---: | :--- |
| `delay` | - Retarder l'émission des valeurs pour simuler un délai réseau dans des tests. <br> - Appliquer un délai entre deux appels d'API pour éviter de saturer le serveur. |
| `finalize` | - Effectuer des actions de nettoyage comme fermer une connexion ou arrêter un indicateur de chargement après la fin du flux, qu'il réussisse ou échoue. <br> - Libérer des ressources (fermer une connexion WebSocket ou une base de données) après l'exécution d'une tâche. |
| `tap` | - Log les valeurs d'un Observable pour le débogage. <br> - Exécuter une fonction de notification à chaque émission. |
| `timeout` | - Annuler une opération ou générer une erreur si l'Observable prend trop de temps pour émettre une valeur (requête API qui trop longue). <br> - Interrompre une interaction utilisateur si aucune réponse n'est reçue dans un délai donné (attendre une confirmation de l'utilisateur). |

## RESSOURCES UTILES
[Reactive How](https://reactive.how/)  

[RX Marble](https://rxmarbles.com/)  

[Think RX](https://thinkrx.io/)  

[Reactive X](https://reactivex.io/)  

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
