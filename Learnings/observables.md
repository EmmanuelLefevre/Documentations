# OBSERVABLES

## INTRODUCTION
Un Observable est un modèle de programmation orienté événements, qui permet de gérer et de réagir aux flux de données asynchrones. Ce concept repose sur l'échange d’informations entre un Observable et un Observer (ou abonné). Un Observable, une fois instancié, reste inactif tant qu'aucun Observer ne s'est souscrit pour écouter ses émissions, ce qui rend son fonctionnement lazy (paresseux). Lorsqu'un Observer souscrit, l'Observable commence alors à émettre les valeurs, erreurs ou signaux de complétion, auxquels l'Observer peut réagir en temps réel. Ce modèle est essentiel pour développer des applications réactives, notamment dans le traitement de données, les interfaces utilisateurs et les appels réseaux.

## DEFINITION
- Un Observable est une fonction pure qui prend en paramètre un Observer.
- Un Observable définit un concept d'échange d'informations autour d'une Souscription.
- Un Observable pur est lazy, c.a.d qu'il ne démarre que lorsqu'un Observer l'écoute.
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
- **Plain Observable**  
Simple stream (mono-stream) => stream unique par Observer, il faut souscrire pour recevoir. L'Observable commence à émettre des valeurs uniquement lorsqu'un Observer s'y abonne. Chaque souscription reçoit une nouvelle émission.  

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

- **Subject**  
Multicast (multi-stream) => stream partagé par les Observers (partage les mêmes émissions), il fournit la même valeur à chacun.  

```typescript
import { Subject } from 'rxjs';

// Création d'un Subject
const subject = new Subject<number>();

// Souscription au Subject
subject.subscribe(value => console.log('Observer 1:', value));

// Émettre une valeur immédiatement
subject.next(Math.random());          // Les Observers reçoivent la valeur émise

// Souscription d'un second Observer
subject.subscribe(value => console.log('Observer 2:', value));
subject.next(Math.random());          // Tous les Observers reçoivent la même valeur
```

- **BehaviorSubject**  
Multicast (multi-stream) + valeur de départ transmise quand on souscrit. Nécessite une valeur initiale et émet la dernière valeur à tout nouvel Observable. Il est utile pour garder un état et émettre toujours la dernière valeur.  

```typescript
import { BehaviorSubject } from 'rxjs';

// Création d'un BehaviorSubject avec une valeur initiale
const behaviorSubject = new BehaviorSubject<number>(Math.random());       // Valeur initiale

// Souscription au BehaviorSubject
behaviorSubject.subscribe(value => console.log('Observer 1:', value));

// Émettre la nouvelle valeur
behaviorSubject.next(Math.random());

// Souscription d'un second Observer
behaviorSubject.subscribe(value => console.log('Observer 2:', value));
// Le second Observer reçoit immédiatement la dernière valeur émise.
```

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

⚠️ Penser à se désabonner avec unsubscribe() afin d'éviter les fuites de mémoires (l'Observable continue à émettre même après la destruction du composant)! ⚠️  
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
| `Hot`| Dès le démarrage de l’Observable. | Non | Oui, si souscrits avant émissions. |

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
