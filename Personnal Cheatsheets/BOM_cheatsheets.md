# BOM

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [OBJECT WINDOW](#object-window)
- [OBJECT NAVIGATOR](#object-navigator)
- [OBJECT LOCATION](#object_location)
- [OBJECT HISTORY](#object-history)
- [OBJECT SCREEN](#object-screen)
- [OBJECT LOCAL STORAGE](#object-local-storage)
- [OBJECT SESSION STORAGE](#object-session-storage)
- [OBJECT PERFORMANCE](#object-performance)
- [OBJECT CONSOLE](#object-console)
- [OBJECT NOTIFICATION](#object-notification)
- [OBJECT GEOLOCATION](#object-geolocation)
- [OBJECT WEB WORKERS](#object-web-workers)

## INTRODUCTION
Le BOM (Browser Object Model) permet aux développeurs d'interagir avec le navigateur web et d'accéder à des fonctionnalités qui ne font pas partie du DOM (Document Object Model).

## OBJECT WINDOW
L'objet global qui représente la fenêtre du navigateur et constitue le point d'entrée principal pour accéder à de nombreuses fonctionnalités.  

| Propriétés | Description |
| :---: | :---: |
| `window.document` | L'objet représentant le DOM du document chargé. |
| `window.location` | Représente l'URL actuelle et permet de la manipuler. |
| `window.history` | Permet d'interagir avec l'historique de navigation. |
| `window.navigator` | Contient des informations sur le navigateur et l'appareil de l'utilisateur. |
| `window.screen` | Informations sur l'écran de l'utilisateur. |
| `window.localStorage` | Stockage persistant de données dans le navigateur. |
| `window.sessionStorage` | Stockage de données pour la session en cours. |
| `window.performance` | Informations sur la performance de la page. |
| `window.console` | Accéder à la console du navigateur pour le débogage. |
| `window.alert()` | Afficher une boîte de dialogue d'alerte. |
| `window.confirm()` | Afficher une boîte de dialogue de confirmation. |
| `window.prompt()` | Afficher une boîte de dialogue pour saisir des informations. |  

| Méthodes | Description |
| :---: | :---: |
| `window.open(url, target)` | Ouvrir une nouvelle fenêtre ou un nouvel onglet. |
| `window.close()` | Fermer la fenêtre actuelle. |
| `window.setTimeout(function, milliseconds)` | Exécuter une fonction après un délai spécifié. |
| `window.clearTimeout(timeoutId)` | Annuler un setTimeout(). |
| `window.setInterval(function, milliseconds)` | Exécuter une fonction à intervalles réguliers. |
| `window.clearInterval(intervalId)` | Annuler un setInterval(). |
| `window.scrollTo(x, y)` | Faire défiler la fenêtre à la position spécifiée. |
| `window.focus()` | Mettre la fenêtre au premier plan. |
| `window.blur()` | Envoyer la fenêtre à l'arrière-plan. |

## OBJECT NAVIGATOR
Fournit des informations sur le navigateur et l'environnement de l'utilisateur.  

| Propriétés | Description |
| :---: | :---: |
| `navigator.userAgent` | Chaîne d'agent utilisateur du navigateur. |
| `navigator.platform` | Plateforme sur laquelle le navigateur est exécuté. |
| `navigator.language` | Langue préférée de l'utilisateur. |
| `navigator.languages` | Tableau des langues préférées de l'utilisateur. |
| `navigator.geolocation` | Permettre d'accéder à la géolocalisation de l'utilisateur. |
| `navigator.onLine` | Indiquer si l'utilisateur est en ligne. |
| `navigator.cookieEnabled` | Indiquer si les cookies sont activés. |

## OBJECT LOCATION
Représente l'URL actuelle du document et permet de la manipuler.  

| Propriétés | Description |
| :---: | :---: |
| `location.href` | URL complète de la page. |
| `location.protocol` | Protocole utilisé (http: ou https:). |
| `location.host` | Nom d'hôte et le port. |
| `location.hostname` | Nom d'hôte sans le port. |
| `location.pathname` | Chemin de l'URL. |
| `location.search` | Chaîne de requête (query string). |
| `location.hash` | Partie de l'URL qui suit le #. |
| `location.origin` | Origine de l'URL (protocole, hôte et port). |  

| Méthodes | Description |
| :---: | :---: |
| `location.assign(url)` | Charger une nouvelle URL. |
| `location.replace(url)` | Remplacer l'URL actuelle sans ajouter à l'historique. |
| `location.reload()` | Recharger la page actuelle. |

## OBJECT HISTORY
Permet d'interagir avec l'historique de navigation de l'utilisateur.  

| Méthodes | Description |
| :---: | :---: |
| `history.back()` | Naviguer à la page précédente. |
| `history.forward()` | Naviguer à la page suivante. |
| `history.go(n)` | Naviguer à une position relative dans l'historique. |
| `history.pushState(state, title, url)` | Ajouter un nouvel état à l'historique. |
| `history.replaceState(state, title, url)` | Remplacer l'état actuel dans l'historique. |

## OBJECT SCREEN
Contient des informations sur la taille et la résolution de l'écran de l'utilisateur.  

| Propriétés | Description |
| :---: | :---: |
| `screen.width` | Largeur de l'écran en pixels. |
| `screen.height` | Hauteur de l'écran en pixels. |
| `screen.availWidth` | Largeur de l'écran disponible (sans la barre de tâches). |
| `screen.availHeight` | Hauteur de l'écran disponible. |
| `screen.colorDepth` | Profondeur de couleur de l'écran. |
| `screen.pixelDepth` | Profondeur de pixel de l'écran. |

## OBJECT LOCAL STORAGE
Permet de stocker des données localement dans le navigateur.  

| Méthodes | Description |
| :---: | :---: |
| `localStorage.setItem(key, value)` | Stocker une paire clé-valeur. |
| `localStorage.getItem(key)` | Récupérer la valeur associée à une clé. |
| `localStorage.removeItem(key)` | Supprimer une paire clé-valeur. |
| `localStorage.clear()` | Supprimer toutes les paires clé-valeur. |

## OBJECT SESSION STORAGE
Similaire à localStorage, mais les données sont stockées pour la durée de la session.  

| Méthodes | Description |
| :---: | :---: |
| `sessionStorage.setItem(key, value)` | Stocker une paire clé-valeur. |
| `sessionStorage.getItem(key)` | Récupérer la valeur associée à une clé. |
| `sessionStorage.removeItem(key)` | Supprimer une paire clé-valeur. |
| `sessionStorage.clear()` | Supprimer toutes les paires clé-valeur. |

## OBJECT PERFORMANCE
Fournit des mesures de performance pour le chargement des pages.  

| Propriétés | Description |
| :---: | :---: |
| `performance.now()` | Renvoyer temps écoulé en ms depuis début de la navigation. |

## OBJECT CONSOLE
Permet d'accéder à la console du navigateur pour le débogage.  

| Méthodes | Description |
| :---: | :---: |
| `console.log()` | Afficher un message dans la console. |
| `console.error()` | Afficher un message d'erreur dans la console. |
| `console.warn()` | Afficher un avertissement dans la console. |
| `console.info()` | Afficher des informations dans la console. |
| `console.table()` | Afficher des données sous forme de tableau dans la console. |

## OBJECT NOTIFICATIONS
Permet d'afficher des notifications à l'utilisateur via l'API des notifications.  

| Méthodes | Description |
| :---: | :---: |
| `Notification.requestPermission()` | Demander permission d'afficher des notifications. |
| `new Notification(title, options)` | Créer une nouvelle notification. |

## OBJECT GEOLOCATION
Permet d'accéder à la position géographique de l'utilisateur.  

| Méthodes | Description |
| :---: | :---: |
| `navigator.geolocation.getCurrentPosition(successCallback, errorCallback)` | Récupérer la position actuelle de l'utilisateur. |
| `navigator.geolocation.watchPosition(successCallback, errorCallback)` | Surveiller les changements de position. |

## OBJECT WEB WORKERS
Permet d'exécuter des scripts en arrière-plan.  

| Méthodes | Description |
| :---: | :---: |
| `new Worker(scriptURL)` | Créer un nouveau worker. |

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the [Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles) one (click on the "Star" button at the top right of the repository page). Thanks 🤗
