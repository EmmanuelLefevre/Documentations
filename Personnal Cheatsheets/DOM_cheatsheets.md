## DOM

## INTRODUCTION
Le DOM (Document Object Model) est une interface de programmation permettant de représenter et d'interagir avec les documents HTML et XML d'une page web. Il organise le document sous forme d'une structure hiérarchique d'objets, où chaque élément, attribut ou morceau de texte devient un noeud. Grâce au DOM, les développeurs peuvent accéder, modifier, ajouter ou supprimer des éléments de la page en temps réel, ce qui rend les interactions dynamiques possibles dans un navigateur. Le DOM permet également de gérer des événements, notamment ceux de la souris et du clavier, pour rendre les pages interactives.

## STRUCTURE DU DOCUMENT
| Elément | Description |
| :---: | :---: |
| `document.documentElement` | Elément racine `<html>` du document. |
| `document.body` | Elément `<body>` du document. |
| `document.head` | Elément `<head>` du document. |
| `document.title` | Définir ou obtenir le titre de la page. |
| `document.URL` | Renvoyer l’URL actuelle du document. |
| `document.referrer` | Renvoyer l’URL de la page d’où l’utilisateur vient. |
| `document.cookie` | Renvoyer ou définit les cookies du document. |
| `document.readyState` | État de chargement actuel du document (`loading`, `interactive`, `complete`). |
| `document.scripts` | Collection des scripts dans le document. |
| `document.forms` | Collection des formulaires dans le document. |
| `document.images` | Collection des images dans le document. |
| `document.anchors` | Collection des ancres (`<a name="...">`) du document. |

## SELECTION D'ELEMENTS
| Méthode | Description |
| :---: | :---: |
| `document.getElementById(id)` | Sélectionner un élément en utilisant son `id`. |
| `document.getElementsByClassName(className)` | Sélectionner tous les éléments ayant la classe spécifiée. |
| `document.getElementsByTagName(tagName)` | Sélectionner tous les éléments d’un type de balise donné. |
| `document.querySelector(selector)`  | Sélectionner le premier élément correspondant au sélecteur CSS. |
| `document.querySelectorAll(selector)`| Sélectionner tous les éléments correspondant au sélecteur CSS. |
| `document.getElementsByName(name)` | Sélectionner tous les éléments ayant un nom spécifique. |
| `element.closest(selector)` | Trouver l'ancêtre le plus proche correspondant au sélecteur donné. |
| `element.matches(selector)` | Vérifier si l’élément correspond à un sélecteur CSS. |
| `element.querySelectorAll(selector)` | Sélectionner tous les éléments descendants correspondant au sélecteur CSS. |

## MANIPULATION D'ELEMENTS
| Propriété/Méthode | Description |
| :---: | :---: |
| `element.innerHTML` | Définir ou obtient le contenu HTML d’un élément. |
| `element.textContent` | Définir ou obtient le contenu texte d’un élément sans balises. |
| `element.setAttribute(name, value)` | Définir un attribut sur un élément avec une valeur spécifique. |
| `element.getAttribute(name)` | Obtenir la valeur d’un attribut d’un élément. |
| `element.removeAttribute(name)` | Supprimer un attribut d’un élément. |
| `element.classList.add(className)` | Ajouter une classe CSS à un élément. |
| `element.classList.remove(className)`| Supprimer une classe CSS d’un élément. |
| `element.appendChild(child)` | Ajouter un élément enfant au parent spécifié. |
| `element.insertBefore(newNode, referenceNode)` | Insérer un noeud avant un autre noeud. |
| `element.replaceWith(newNode)` | Remplacer l’élément par un autre noeud. |
| `element.cloneNode(deep)` | Créer une copie de l’élément (en profondeur si `deep` est `true`). |
| `element.hasAttribute(name)` | Vérifier si un attribut est présent sur l’élément. |
| `element.innerText` | Obtenir ou définir le texte visible d’un élément. |
| `element.outerHTML` | Obtenir ou définir l’élément lui-même, avec son contenu HTML. |
| `element.remove()` | Supprimer l'élément de son parent. |

## GESTION D'EVENEMENTS
### Mouse Events
| Evénement | Description |
| :---: | :---: |
| `click` | Se déclenche lorsque l’utilisateur clique sur l’élément. |
| `dblclick` | Se déclenche lors d’un double-clic sur l’élément. |
| `mouseover` | Se déclenche lorsque la souris survole l’élément. |
| `mouseout` | Se déclenche lorsque la souris quitte l’élément. |
| `mousedown` | Se déclenche lorsque le bouton de la souris est enfoncé. |
| `mouseup` | Se déclenche lorsque le bouton de la souris est relâché. |

### Keyboard Events
| Evénement | Description |
| :---: | :---: |
| `keydown` | Se déclenche lorsque l’utilisateur appuie sur une touche. |
| `keyup` | Se déclenche lorsque l’utilisateur relâche une touche. |
| `keypress` | Se déclenche lors de l’appui sur une touche produisant un caractère. |

### Window Events
| Evénement | Description |
| :---: | :---: |
| `resize` | Se déclenche lorsque la taille de la fenêtre change. |
| `scroll` | Se déclenche lorsque la fenêtre est défilée. |
| `load` | Se déclenche lorsque le document et tous ses sous-ressources sont entièrement chargés. |
| `unload` | Se déclenche lorsque la page est sur le point d’être déchargée. |

### Ecran Tactile Events
| Evénement | Description |
| :---: | :---: |
| `touchstart` | Se déclenche lorsqu’un point de contact est posé. |
| `touchmove` | Se déclenche lorsque le point de contact se déplace. |
| `touchend` | Se déclenche lorsque le point de contact est levé. |
| `touchcancel` | Se déclenche si l’événement est annulé (interruption par une autre interface). |

## FONCTIONS UTILITAIRES DE FENETRE
| Méthodes | Description |
| :---: | :---: |
| `window.open(url, target)` | Ouvrir nouvelle fenêtre ou nouvel onglet avec une URL spécifiée. |
| `window.close()` | Fermer la fenêtre ou l’onglet actuel (si autorisé par le navigateur). |
| `window.alert(message)` | Afficher alerte avec message à l’utilisateur. |
| `window.confirm(message)` | Afficher boîte de dialogue pour obtenir une confirmation de l’utilisateur. |
| `window.prompt(message, default)` | Afficher une invite pour une saisie utilisateur. |
| `window.scrollTo(x, y)` | Faire défiler la fenêtre jusqu'aux coordonnées `x` et `y`. |
| `window.setTimeout(function, delay)` | Exécuter fonction après délai spécifié en millisecondes. |
| `window.setInterval(function, interval)` | Exécuter fonction périodiquement avec intervalle spécifié. |
| `window.location` | Obtenir ou définir l'URL actuelle. |
| `window.history.back()` | Revenir à la page précédente dans l'historique. |

## GESTION DU STYLE
| Propriété/Méthode | Description |
| :---: | :---: |
| `element.style.property` | Permet de modifier une propriété CSS directement. |
| `element.classList.toggle(className)` | Activer/désactiver une classe pour un élément. |
| `getComputedStyle(element)` | Renvoyer objet représentant les styles calculés d’un élément. |
| `element.classList.contains(className)` | Vérifier si l’élément contient une certaine classe. |
| `element.style.cssText` | Définir plusieurs propriétés CSS en une seule fois. |
| `element.classList.replace(oldClass, newClass)` | Remplacer une classe par une autre pour un élément. |

## GESTION DE L'ARBORESCENCE
| Méthode | Description |
| :---: | :---: |
| `document.createElement(tagName)` | Créer un nouvel élément du type spécifié. |
| `document.createTextNode(text)` | Créer un noeud de texte contenant le texte spécifié. |
| `element.appendChild(node)` | Ajouter un noeud en tant qu’enfant de l’élément. |
| `element.removeChild(node)` | Supprimer un enfant de l’élément. |
| `element.replaceChild(newNode, oldNode)` | Remplacer un noeud enfant par un autre. |
| `element.childNodes` | Collection de tous les noeuds enfants d’un élément (noeuds texte aussi). |
| `element.firstChild` | Premier noeud enfant de l’élément. |
| `element.lastChild` | Dernier noeud enfant de l’élément. |
| `element.nextSibling` | Noeud suivant dans l’arborescence. |
| `element.previousSibling` | Noeud précédent dans l’arborescence. |

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
