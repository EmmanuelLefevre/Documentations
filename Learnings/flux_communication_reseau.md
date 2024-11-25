# CHAINE DE REQUETE HTTP ENTRE CLIENT-SERVEUR AVEC RESOLUTION DNS
| Etape                               | Description                           |
| :---------------------------------: | :-----------------------------------: |
| Client browser =>                   | L'utilisateur initie une requête HTTP depuis le navigateur |
| HTTP/HTTPS/etc (Header/Payload) =>  | La requête HTTP/HTTPS avec en-têtes et payload est envoyée |
| carte réseau =>                     | La carte réseau du client envoie les paquets vers le routeur |
| router =>                           | Le routeur transfère les paquets vers le FAI |
| FAI =>                              | Le FAI relaye la requête |
| server DNS maitre =>                | Le serveur DNS maître reçoit la requête de résolution de nom |
| resolve name server =>              | Le serveur de nom effectue une résolution en cascade |
| root name server =>                 | Le serveur racine DNS est contacté pour trouver l'autorité |
| tld name server =>                  | Le serveur DNS du domaine de premier niveau (TLD) est interrogé |
| server listen port =>               | Le serveur écoute sur le port spécifié (ex: 80 pour HTTP, 443 pour HTTPS) |
| content =>                          | Le contenu du serveur est traité et envoyé au client |
| web server response =>              | Le serveur web envoie la réponse HTTP au client |
| client browser                      | Le navigateur reçoit et affiche la réponse |

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
