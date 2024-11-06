# OBSERVABLES

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [DEFINITION](#definition)
- [METHODES DE NOTIFICATION](#methodes-de-notification)
- [TYPES](#types)
- [Plain Observable](#plain-observable)

## INTRODUCTION
Un Observable est un modèle de programmation orienté événements, qui permet de gérer et de réagir aux flux/stream de données asynchrones. Ce concept repose sur l'échange d’informations entre un Observable et un Observer (ou abonné).  
Lorsqu'un Observer souscrit, l'Observable commence alors à émettre des valeurs (sauf dans le cas d'un Observable Hot où l'Observable émet dès son instanciation), ainsi que des erreurs ou signaux de complétion, auxquels l'Observer peut réagir en temps réel.  
Ce modèle est essentiel pour développer des applications réactives, notamment dans le traitement de données, les interfaces utilisateurs et les appels réseaux.

## DEFINITION
- Un Observable est une fonction pure qui prend en paramètre un Observer.
- Un Observable définit un concept d'échange d'informations autour d'une Souscription.
- Un Observable pur est lazy, c.a.d qu'une fois instancié il reste inactif tant qu'aucun Observer ne s'est souscrit pour écouter ses émissions.
- Un Observable peut être écouté et l'écoute stoppée à volonté.

## METHODES DE NOTIFICATION
Les Observables utilisent principalement quatre méthodes pour transmettre des informations à leurs Observers, chacune remplissant un rôle distinct dans la gestion du flux de données.

- **next(value)**  
La méthode next est utilisée pour transmettre les valeurs au fur et à mesure de leur production. Chaque fois que l’Observable émet une valeur, elle est passée en argument de next, et chaque Observer souscrit recevra cette valeur en temps réel. Les Observers peuvent alors réagir à chaque émission, permettant un traitement continu et fluide des données reçues.

- **error(error)**  
La méthode error est appelée lorsque l'Observable rencontre une erreur irrécupérable. Cette méthode arrête immédiatement le flux de données en cours et signale l’erreur aux Observers. Ceux-ci peuvent alors gérer cette erreur dans une section de code dédiée, et l'Observable ne produira plus d'autres valeurs ni d'autres notifications après cette erreur.

- **throw(error)**  
La méthode throw peut être utilisée pour signaler une erreur de manière explicite dans un Observable ou dans un flux de données. Bien que throw ne soit pas toujours directement appelée dans un Observable, elle est souvent utilisée pour gérer les exceptions lors de la création des Observables personnalisés. Elle permet aux développeurs de définir des comportements d'erreur spécifiques.

- **complete()**  
La méthode complete est appelée lorsqu’un Observable a terminé d’émettre des valeurs, indiquant qu’il n’y aura plus de données futures à transmettre. Cette notification met fin à la souscription et signifie aux Observers que le flux est arrivé à son terme sans erreurs. Elle est particulièrement utile pour indiquer la fin de tâches finies, comme des appels d'API ou des traitements de données.  

## TYPES
### **Plain Observable**  
Simple-cast (mono-stream) => stream unique par Observer, chaque souscription crée un flux indépendant et il faut souscrire pour recevoir.  
Cet Observable commence à émettre des valeurs uniquement lorsqu'un Observer s'y abonne et chaque souscription reçoit une nouvelle émission.  Utile pour des flux uniques, indépendants pour chaque abonné.  

```typescript
import { Observable } from 'rxjs';

// Création d'un Plain Observable qui émet une valeur aléatoire à chaque souscription
const coldObservable = new Observable<number>((observer) => {
  observer.next(Math.random());     // Produit une nouvelle valeur
  observer.complete();              // Termine l'Observable
});

// Souscription à l'Observable
coldObservable.subscribe(value => console.log('Observer 1:', value));
coldObservable.subscribe(value => console.log('Observer 2:', value));
// Chaque Observer reçoit une valeur différente car l'Observable est "froid".
```

### **Subject**  
Multi-cast (multi-stream) => stream partagé par les Observers (partage le même flux/émissions), il fournit la même valeur à chacun.  
Le Subject est un Observable spécial qui permet de partager les mêmes émissions entre tous les abonnés. Il est également utilisé pour transformer un flux mono-stream en multicast, permettant à plusieurs Observers de recevoir les mêmes valeurs au même moment.  

```typescript
import { Subject } from 'rxjs';

// Création d'un Subject
const subject = new Subject<number>();

// Souscription au Subject
subject.subscribe(value => console.log('Observer 1:', value));

// Émettre une valeur immédiatement
subject.next(Math.random());     // Les Observers reçoivent la valeur émise

// Souscription d'un second Observer
subject.subscribe(value => console.log('Observer 2:', value));
subject.next(Math.random());      // Tous les Observers reçoivent la même valeur
```

### **BehaviorSubject**  
Multi-cast (multi-stream) => partage le même flux de données entre les abonnés avec une valeur initiale (valeur de départ transmise quand on souscrit + émet la dernière valeur à tout nouvel Observable).  
Le BehaviorSubject conserve la dernière valeur émise et l'envoie immédiatement à tout nouvel abonné, même s'il s'est souscrit après des émissions.  
Il est utile pour garder un état et émettre toujours la dernière valeur et c’est un excellent choix pour stocker et émettre des états partagés.  

```typescript
import { BehaviorSubject } from 'rxjs';

// Création d'un BehaviorSubject avec une valeur initiale
const behaviorSubject = new BehaviorSubject<number>(Math.random());     // Valeur initiale

// Souscription au BehaviorSubject
behaviorSubject.subscribe(value => console.log('Observer 1:', value));

// Émettre la nouvelle valeur
behaviorSubject.next(Math.random());

// Souscription d'un second Observer
behaviorSubject.subscribe(value => console.log('Observer 2:', value));
// Le second Observer reçoit immédiatement la dernière valeur émise.
```

### **ReplaySubject**  
Multi-cast (multi-stream) => émet les valeurs passées aux abonnés actuels et futurs jusqu'à une limite spécifiée.  
Le ReplaySubject stocke un certain nombre de valeurs passées (ou toutes si non limité) et les envoie à tout nouvel abonné dès sa souscription. Idéal pour fournir un historique à ceux qui s’abonnent après certaines émissions.  

```typescript
import { AsyncSubject } from 'rxjs';

// Création d'un AsyncSubject
const asyncSubject = new AsyncSubject<number>();

// Souscriptions
asyncSubject.subscribe(value => console.log('Observer 1:', value));

asyncSubject.next(1);
asyncSubject.next(2);
asyncSubject.next(3);     // Seule cette valeur sera envoyée lors du complete()
asyncSubject.complete();

asyncSubject.subscribe(value => console.log('Observer 2:', value));     // Reçoit également 3
```

### **AsyncSubject**  
Multi-cast (multi-stream) => envoie uniquement la dernière valeur émise à la complétion de l’observable.  
L’AsyncSubject ne diffuse une valeur qu’une fois l’observable complété et fournit uniquement cette dernière valeur à tous les abonnés. Très utile dans les cas où l'on ne souhaite que diffuser un résultat final unique (ex: réponse d'API).  

```typescript
import { AsyncSubject } from 'rxjs';

// Création d'un AsyncSubject
const asyncSubject = new AsyncSubject<number>();

// Souscriptions
asyncSubject.subscribe(value => console.log('Observer 1:', value));

asyncSubject.next(1);
asyncSubject.next(2);
asyncSubject.next(3);     // Seule cette valeur sera envoyée lors du complete()
asyncSubject.complete();

asyncSubject.subscribe(value => console.log('Observer 2:', value     // Reçoit également 3
```

### **ReplayObservable**  
Simple-cast (mono-stream) => chaque abonné reçoit un flux indépendant qui rejoue certaines valeurs historiques.  
Bien que semblable au ReplaySubject, un ReplayObservable fournit un historique défini aux abonnés, mais reste un mono-stream. Il émet des valeurs spécifiques jusqu'à ce qu'il atteigne un certain point.  
Ce type d'observable est moins courant mais utile pour conserver un historique de valeurs sur une période définie ou pour créer des flux personnalisés avec des valeurs répétées.  

```typescript
import { of } from 'rxjs';
import { replay } from 'rxjs/operators';

// Création d'un ReplayObservable avec un historique
const replayObservable = of(1, 2, 3).pipe(replay(2));

// Chaque souscription reçoit un flux unique
replayObservable.subscribe(value => console.log('Observer 1:', value));     // Reçoit [1, 2, 3]
replayObservable.subscribe(value => console.log('Observer 2:', value));     // Reçoit [1, 2, 3]
```

### **ConnectableObservable**  
Multi-cast (multi-stream) => les abonnés partagent un flux déclenché manuellement.
Un ConnectableObservable est un type d’Observable qui reste inactif (ne commence pas automatiquement à émettre des valeurs lors de la souscription) jusqu'à l'appel de la méthode connect().  
Ce type de multicast est utile pour contrôler manuellement le moment où le flux de données démarre pour tous les abonnés en même temps.  

```typescript
import { interval, ConnectableObservable } from 'rxjs';
import { publish } from 'rxjs/operators';

// Création d'un ConnectableObservable
const connectableObservable = interval(1000).pipe(publish()) as ConnectableObservable<number>;

// Souscriptions
connectableObservable.subscribe(value => console.log('Observer 1:', value));
connectableObservable.subscribe(value => console.log('Observer 2:', value));

// Démarre la diffusion pour tous les abonnés
connectableObservable.connect();
```

### **Tableau récapitulatif**  
| Type | Stream | Description |
| :---: | :---: | :---: |
| `Plain Observable` | Mono-stream | Flux indépendant pour chaque abonné. |
| `Subject` | Multi-stream | Flux partagé pour tous les abonnés. |
| `BehaviorSubject` | Multi-stream | Flux partagé avec valeur initiale, conserve la dernière valeur. |
| `ReplaySubject` | Multi-stream | Flux partagé avec historique des valeurs émises. |
| `AsyncSubject` | Multi-stream | Envoie uniquement la dernière valeur à la complétion. |
| `ReplayObservable` | Mono-stream | Flux individuel avec historique pour chaque abonné. |
| `ConnectableObservable` | Multi-stream | Flux partagé démarré manuellement pour tous les abonnés. |

## SOUSCRIPTION
Lorsqu'un Observer s'abonne à un Observable, il utilise l'une des deux méthodes suivantes :  
### Callback
```javascript
const subscription = observable.subscribe(
  value => console.log(value),       // next
  error => console.log(error),       // error
  () => console.log("complete.."),   // complete
);
```
### Object
```javascript
const subscription = observable.subscribe({
  next: value => console.log(value),           // next
  error: error => console.log(error),          // error
  complete: () => console.log("complete.."),   // complete
)};
```

⚠️ Pensez à vous désabonner avec unsubscribe() afin d'éviter les fuites de mémoire, car l'Observable peut continuer à émettre même après la fin du cycle de vie du composant qui l'utilise !  
**Exception :** lors de l'utilisation du pipe async dans un template Angular. En effet celui-ci gère automatiquement la souscription et le désabonnement, lorsqu'un composant Angular est détruit, le pipe async se désabonne automatiquement de l'Observable.

## HOT/COLD
En RxJS, les concepts de "hot" et "cold" Observables permettent de comprendre comment un Observable émet des données et de quelle façon celles-ci sont perçues par les Observers qui s'y souscrivent.  

### Cold
Un Cold Observable est un Observable paresseux (lazy), qui ne commence à émettre des données que lorsque l’on y souscrit.  
Chaque Observer obtient ainsi sa propre instance du flux de données, ce qui signifie que chaque nouvelle souscription démarre un flux indépendant des autres.  
Les Cold Observables sont souvent utilisés pour des opérations qui doivent être exécutées à chaque nouvelle souscription, comme les appels HTTP ou la lecture d’un fichier.  

**Exemple :**  
```typescript
import { Observable } from 'rxjs';

const coldObservable = new Observable<number>((observer) => {
  observer.next(Math.random()); // Produit une nouvelle valeur à chaque souscription
  observer.complete();
});

coldObservable.subscribe(value => console.log('Observer 1:', value));
coldObservable.subscribe(value => console.log('Observer 2:', value));
// Les deux Observers recevront des valeurs différentes, car le flux est relancé pour chaque souscription.
```

### Hot
Un Hot Observable est un Observable qui commence à émettre des données immédiatement, indépendamment des souscriptions. Tous les Observers qui s’y souscrivent reçoivent les mêmes valeurs au même moment.  
Dans ce cas, l'Observable "chauffe" (d'où le terme hot) avant l’arrivée des Observers, et ces derniers peuvent manquer des valeurs s’ils se souscrivent tardivement.  
Les Hot Observables sont souvent utilisés dans des situations où les données sont générées de manière continue et doivent être partagées, comme les événements utilisateur, les WebSockets, ou les flux de capteurs.  

**Exemple :**  
```typescript
import { Subject } from 'rxjs';

const subject = new Subject<number>();

subject.subscribe(value => console.log('Observer 1:', value));

subject.next(Math.random()); // Valeur émise immédiatement

subject.subscribe(value => console.log('Observer 2:', value));
// Les deux Observers recevront la même valeur si souscrits avant l'émission.
```

### Différences clés entre Cold et Hot Observables
| Type | Emission | Flux indépendant par Observer | Synchronisation des valeurs |
| :---: | :---: | :---: | :---: |
| `Cold` | Au moment de la souscription. | Oui | Non |
| `Hot`| Dès le démarrage de l’Observable. | Non | Oui, si souscrites avant émissions. |

### **Tableau récapitulatif**  
| Type | Cold/Hot |
| :---: | :---: |
| `Plain Observable` | Cold |
| `Subject` | Hot |
| `BehaviorSubject` | Hot |
| `ReplaySubject` | Hot |
| `AsyncSubject` | Hot |
| `ReplayObservable` | Cold|
| `ConnectableObservable` | Hot |

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
