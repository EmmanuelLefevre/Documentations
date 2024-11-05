# HTTP STATUS CODE

## CATÉGORIES
| Code | Description |
| :---: | :---: |
|**1XX**|Demandes d'information|
|**2XX**|Demandes réussies|
|**3XX**|Redirections|
|**4XX**|Erreurs client|
|**5XX**|Erreurs serveur|

## LISTE COMPLÈTE
| Code | Nom | Description |
| :---: | :---: | :---: |
|`100`|Continue|Tout va bien jusqu'à présent et le client doit continuer avec la demande ou l'ignorer s'il est déjà terminé|
|`101`|Switching Protocols|Le client a demandé au serveur de changer de protocole et le serveur a accepté de le faire|
|`102`|Processing|Le serveur a reçu et traite la demande, mais n'a pas encore de réponse finale|
|`103`|Early Hints|Utilisé pour renvoyer certains en-têtes de réponse avant le message HTTP final|
|`200`|OK|Demande réussie|
|`201`|Created|Le serveur a reconnu la ressource créée|
|`202`|Accepted|La demande du client a été reçue mais le serveur est encore en train de la traiter|
|`203`|Non-Authoritative Information|La réponse que le serveur a envoyée au client n'est pas la même que celle qu'il a envoyée|
|`204`|No Content|Il n'y a pas de contenu à envoyer pour cette demande|
|`205`|Reset Content|Indique à l'agent utilisateur de réinitialiser le document qui a envoyé cette demande|
|`206`|Partial Content|Ce code de réponse est utilisé lorsque l'en-tête de plage est envoyé par le client pour demander uniquement une partie d'une ressource|
|`207`|Multi-Status|Transmet des informations sur plusieurs ressources, pour des situations où plusieurs codes d'état peuvent être appropriés|
|`208`|Already Reported|Les membres d'un lien DAV ont déjà été énumérés dans une partie précédente de la réponse multi-état|
|`226`|IM Used|IM est une extension spécifique du protocole HTTP. L'extension permet à un serveur HTTP d'envoyer des diffs (changements) de ressources aux clients|
|`300`|Multiple Choices|La demande a plus d'une réponse possible. L'agent utilisateur doit en choisir une|
|`301`|Moved Permanently|L'URL de la ressource demandée a changé de manière permanente. La nouvelle URL est donnée dans la réponse|
|`302`|Found|Ce code de réponse signifie que l'URI de la ressource demandée a changé temporairement|
|`303`|See Other|Le serveur a envoyé cette réponse pour diriger le client à obtenir la ressource demandée à un autre URI avec une demande GET|
|`304`|Not Modified|Il indique au client que la réponse n'a pas été modifiée, donc le client peut continuer à utiliser la même version mise en cache de la réponse|
|`305`|Use Proxy|Défini dans une version précédente de la spécification HTTP pour indiquer qu'une réponse demandée doit être accessible par un proxy. (abandonné)|
|`307`|Temporary Redirect|Le serveur envoie cette réponse pour diriger le client à obtenir la ressource demandée à un autre URI avec la même méthode que celle utilisée dans la demande précédente|
|`308`|Permanent Redirect|Cela signifie que la ressource est maintenant située de manière permanente à un autre URI, spécifié par l'en-tête de réponse HTTP Location :|
|`400`|Bad Request|Le serveur n'a pas pu comprendre la demande|
|`401`|Unauthorized|Le client ne s'est pas authentifié|
|`402`|Payment Required|Ce code de réponse est réservé à un usage futur. Le but initial de ce code était de l'utiliser pour des systèmes de paiement numérique, cependant ce code d'état est très rarement utilisé et aucune convention standard n'existe|
|`403`|Forbidden|Le client n'a pas les droits d'accès au contenu|
|`404`|Not Found|Le serveur ne peut pas trouver la ressource demandée|
|`405`|Method Not Allowed|La méthode de demande est connue par le serveur mais n'est pas prise en charge par la ressource cible|
|`406`|Not Acceptable|La réponse ne correspond pas aux critères fournis par le client|
|`407`|Proxy Authentication Required|C'est similaire à 401 Unauthorized mais l'authentification doit être effectuée par un proxy|
|`408`|Request Timeout|Cette réponse est envoyée sur une connexion inactive par certains serveurs, même sans aucune demande préalable du client|
|`409`|Conflict|Cette réponse est envoyée lorsqu'une demande entre en conflit avec l'état actuel du serveur|
|`410`|Gone|Cette réponse est envoyée lorsque le contenu demandé a été définitivement supprimé du serveur, sans adresse de transfert|
|`411`|Length Required|Le serveur a rejeté la demande car le champ d'en-tête Content-Length n'est pas défini et le serveur l'exige|
|`412`|Precondition Failed|L'accès à la ressource cible a été refusé|
|`413`|Payload Too Large|L'entité de la demande est plus grande que les limites définies par le serveur|
|`414`|Request-URI Too Long|L'URI demandé par le client est plus long que ce que le serveur est prêt à interpréter|
|`415`|Unsupported Media Type|Le format multimédia n'est pas pris en charge par le serveur|
|`416`|Requested Range Not Satisfiable|La plage spécifiée par le champ d'en-tête Range dans la demande ne peut pas être satisfaite|
|`417`|Expectation Failed|L'attente indiquée par le champ d'en-tête de demande Expect ne peut pas être satisfaite par le serveur|
|`418`|I'm a teapot|Le serveur refuse la tentative de préparer du café avec une théière|
|`421`|Misdirected Request|La demande a été dirigée vers un serveur qui n'est pas en mesure de produire une réponse|
|`422`|Unprocessable Entity|La demande était bien formée mais n'a pas pu être suivie en raison d'erreurs sémantiques|
|`423`|Locked|La ressource à laquelle on accède est verrouillée|
|`424`|Failed Dependency|La demande a échoué en raison de l'échec d'une demande précédente|
|`426`|Upgrade Required|Le serveur refuse d'exécuter la demande en utilisant le protocole actuel mais pourrait être disposé à le faire après que le client passe à un autre protocole|
|`428`|Precondition Required|Cette réponse est destinée à prévenir le problème de "mise à jour perdue", où un client obtient l'état d'une ressource, le modifie et le remet au serveur, tandis qu'un tiers a modifié l'état sur le serveur, entraînant un conflit|
|`429`|Too Many Requests|L'utilisateur a envoyé trop de demandes dans un laps de temps donné|
|`431`|Request Header Fields Too Large|Le serveur ne peut pas traiter la demande car ses champs d'en-tête sont trop grands|
|`444`|Connection Closed Without Response|La connexion s'est ouverte, mais aucune donnée n'a été écrite|
|`451`|Unavailable For Legal Reasons|L'agent utilisateur a demandé une ressource qui ne peut pas être légalement fournie (comme une page web censurée par un gouvernement)|
|`499`|Client Closed Request|Le client a fermé la connexion, bien que le serveur ait déjà traité la demande|
|`500`|Internal Server Error|Le serveur a rencontré une situation qu'il ne sait pas comment gérer|
|`501`|Not Implemented|La méthode de demande n'est pas prise en charge par le serveur et ne peut pas être traitée|
|`502`|Bad Gateway|Cette réponse d'erreur signifie que le serveur, tout en agissant comme une passerelle pour obtenir une réponse nécessaire pour traiter la demande, a obtenu une réponse invalide|
|`503`|Service Unavailable|Le serveur n'est pas prêt à traiter la demande|
|`504`|Gateway Timeout|Cette réponse d'erreur est donnée lorsque le serveur agit comme une passerelle et ne peut pas obtenir une réponse à temps|
|`505`|HTTP Version Not Supported|La version HTTP utilisée dans la demande n'est pas prise en charge par le serveur|
|`506`|Variant Also Negotiates|La ressource variant choisie est configurée pour s'engager dans une négociation de contenu transparente elle-même, et n'est donc pas un point de terminaison approprié dans le processus de négociation|
|`507`|Insufficient Storage|La méthode n'a pas pu être exécutée sur la ressource car le serveur est incapable de stocker la représentation nécessaire pour compléter la demande avec succès|
|`508`|Loop Detected|Le serveur a détecté une boucle infinie lors du traitement de la demande|
|`510`|Not Extended|D'autres extensions à la demande sont nécessaires pour que le serveur puisse l'exécuter|
|`511`|Network Authentication Required|Indique que le client doit s'authentifier pour obtenir l'accès au réseau|
|`599`|Network Connect Timeout Error|La connexion a expiré en raison d'un serveur surchargé, d'une erreur matérielle ou d'une erreur d'infrastructure|

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
