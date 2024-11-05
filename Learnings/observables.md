# OBSERVABLES

## INTRODUCTION
Un Observable est un modèle de programmation orienté événements, qui permet de gérer et de réagir aux flux de données asynchrones. Ce concept repose sur l'échange d’informations entre un Observable et un Observer (ou abonné). Un Observable, une fois instancié, reste inactif tant qu'aucun observer ne s'est souscrit pour écouter ses émissions, ce qui rend son fonctionnement lazy (paresseux). Lorsqu'un observer souscrit, l'Observable commence alors à émettre les valeurs, erreurs ou signaux de complétion, auxquels l'observer peut réagir en temps réel. Ce modèle est essentiel pour développer des applications réactives, notamment dans le traitement de données, les interfaces utilisateurs et les appels réseaux.

## DEFINITION
- Un Observable est une fonction pure qui prend en paramètre un observer.
- Un Observable définit un concept d'échange d'informations autour d'une Souscription.
- Un Observable pur est lazy, c.a.d qu'il ne démarre que lorsqu'un observer l'écoute.
- Un Observable peut être écouté et l'écoute stoppée à volonté.

## TYPES
- **Observable plain**  
Simple stream (mono-stream), un stream unique par observer. Il faut souscrire pour recevoir =>  
new Observable()
- **Subject**  
Multicast (multi-stream), un stream est partagé par les observers. Il fournit la même valeur à chacun =>  
new Subject()
- **BehaviorSubject**  
Multicast (multi-stream) avec une valeur de départ transmise quand on souscrit =>  
new BehaviorSubject()

## METHODES DE NOTIFICATION
Les Observables utilisent principalement quatre méthodes pour transmettre des informations à leurs observers, chacune remplissant un rôle distinct dans la gestion du flux de données.

- **next(value)**  
La méthode next est utilisée pour transmettre les valeurs au fur et à mesure de leur production. Chaque fois que l’Observable émet une valeur, elle est passée en argument de next, et chaque observer souscrit recevra cette valeur en temps réel. Les observers peuvent alors réagir à chaque émission, permettant un traitement continu et fluide des données reçues.

- **error(error)**  
La méthode error est appelée lorsque l'Observable rencontre une erreur irrécupérable. Cette méthode arrête immédiatement le flux de données en cours et signale l’erreur aux observers. Ceux-ci peuvent alors gérer cette erreur dans une section de code dédiée, et l'Observable ne produira plus d'autres valeurs ni d'autres notifications après cette erreur.

- **throw(error)**  
La méthode throw peut être utilisée pour signaler une erreur de manière explicite dans un Observable ou dans un flux de données. Bien que throw ne soit pas toujours directement appelée dans un Observable, elle est souvent utilisée pour gérer les exceptions lors de la création des Observables personnalisés. Elle permet aux développeurs de définir des comportements d'erreur spécifiques.

- **complete()**  
La méthode complete est appelée lorsqu’un Observable a terminé d’émettre des valeurs, indiquant qu’il n’y aura plus de données futures à transmettre. Cette notification met fin à la souscription et signifie aux observers que le flux est arrivé à son terme sans erreurs. Elle est particulièrement utile pour indiquer la fin de tâches finies, comme des appels d'API ou des traitements de données.

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
