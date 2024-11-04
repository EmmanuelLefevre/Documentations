# DOCKER

## INTRODUCTION
Docker est une plateforme pour lancer des applications dans des conteneurs logiciels.

## COMMANDES
| Command | Description |
| :---: | :---: |
| `docker run <image>` | Démarrer un nouveau conteneur à partir d'une image |
| `docker run -it <image>` | Démarrer un nouveau conteneur en mode interactif |
| `docker run --rm <image>` | Démarrer un nouveau conteneur et le supprimer à sa sortie |
| `docker run -it node:16` | Démarrer un nouveau conteneur Node V16 |
| `docker run -it node-test:latest bash` | Démarrer un conteneur et ouvrir un terminal interactif dans celui-ci |
| `docker run -it -d <image>` | Faire tourner en arrière plan une image |
| `docker create <image>` | Créer un nouveau conteneur |
| `docker start <container>` | Démarrer un conteneur |
| `docker stop <container id>` | Arrêter un conteneur spécifique |
| `docker kill <container>` | Tuer (SIGKILL) un conteneur |
| `docker restart <container>` | Arrêter et redémarrer un conteneur de manière gracieuse |
| `docker pause <container>` | Suspendre un conteneur |
| `docker unpause <container>` | Reprendre un conteneur |
| `docker rm <container>` | Détruire un conteneur |

## GESTION DES CONTENEURS
| Command | Description |
| :---: | :---: |
| `docker stop $(docker ps -q)` | Arrêter tous les conteneurs en cours d'exécution |
| `docker stop $(docker ps -a -q)` | Arrêter tous les conteneurs, arrêtés et en cours d'exécution |
| `docker kill $(docker ps -q)` | Tuer tous les conteneurs en cours d'exécution |
| `docker kill $(docker ps -a -q)` | Tuer tous les conteneurs, arrêtés et en cours d'exécution |
| `docker restart $(docker ps  -q)` | Redémarrer tous les conteneurs en cours d'exécution |
| `docker restart $(docker ps -a -q)` | Redémarrer tous les conteneurs arrêtés |
| `docker rm $(docker ps  -q)` | Détruire tous les conteneurs en cours d'exécution |
| `docker rm $(docker ps -a -q)` | Détruire tous les conteneurs arrêtés |
| `docker pause $(docker ps  -q)` | Suspendre tous les conteneurs en cours d'exécution |
| `docker pause $(docker ps -a -q)` | Suspendre tous les conteneurs arrêtés |
| `docker start $(docker ps  -q)` | Démarrer tous les conteneurs en cours d'exécution |
| `docker start $(docker ps -a -q)` | Démarrer tous les conteneurs arrêtés |
| `docker rm -vf $(docker ps -a -q)` | Supprimer tous les conteneurs, y compris leurs volumes |
| `docker rmi -f $(docker images -a -q)` | Supprimer toutes les images |
| `docker system prune` | Supprimer toutes les images, conteneurs, caches et volumes inutilisés |
| `docker system prune -a` | Supprimer toutes les images utilisées et non utilisées |
| `docker system prune --volumes` | Supprimer tous les volumes Docker |

## INSPECTION DES CONTENEURS
| Command | Description |
| :---: | :---: |
| `docker ps` | Lister les conteneurs en cours d'exécution |
| `docker ps --all` | Lister tous les conteneurs, y compris ceux arrêtés |
| `docker logs <container>` | Afficher la sortie d'un conteneur |
| `docker logs -f <container>` | Suivre la sortie d'un conteneur |
| `docker logs -f <container> 2>&1 | grep string-to-search` | Suivre les logs du conteneur et chercher une chaîne spécifique |
| `docker top <container>` | Lister les processus en cours dans un conteneur |
| `docker diff <container>` | Afficher les différences avec l'image (fichiers modifiés) |
| `docker inspect <container>` | Afficher les informations d'un conteneur (format JSON) |

## EXÉCUTION DE COMMANDES
| Command | Description |
| :---: | :---: |
| `docker attach <container>` | S'attacher à un conteneur |
| `docker cp <container>:<container-path> <host-path>` | Copier des fichiers du conteneur |
| `docker cp <host-path> <container>:<container-path>` | Copier des fichiers dans le conteneur |
| `docker export <container>` | Exporter le contenu du conteneur (archive tar) |
| `docker exec <container>` | Exécuter une commande à l'intérieur d'un conteneur |
| `docker exec -it <container> /bin/bash` | Ouvrir un shell interactif à l'intérieur d'un conteneur (utiliser /bin/sh si bash n'est pas présent) |
| `docker wait <container>` | Attendre la terminaison du conteneur et retourner le code de sortie |

## IMAGES
| Command | Description |
| :---: | :---: |
| `docker image ls` | Lister toutes les images locales |
| `docker history <image>` | Afficher l'historique de l'image |
| `docker inspect <image>` | Afficher les informations (format JSON) |
| `docker tag <image> <tag>` | Taguer une image |
| `docker commit <container> <image>` | Créer une image à partir d'un conteneur |
| `docker import <url>` | Créer une image à partir d'une archive tar |
| `docker rmi <image>` | Supprimer des images |
| `docker pull node` | Télécharger image Node.js par défaut |
| `docker pull node:16` | Télécharger version 16 de Node.js |
| `docker pull node:latest` | Télécharger dernière version de Node.js |
| `docker pull <user>/<repository>:<tag>` | Tirer une image d'un registre |
| `docker push <user>/<repository>:<tag>` | Pousser une image vers un registre |
| `docker search <test>` | Rechercher une image sur le registre officiel |
| `docker login` | Se connecter à un registre |
| `docker logout` | Se déconnecter d'un registre |
| `docker save <user>/<repository>:<tag>` | Exporter une image/répertoire en tant qu'archive tar |
| `docker load` | Charger des images à partir d'une archive tar |
| `docker build -t node-test:latest .` | Construire une image Docker à partir d'un fichier Dockerfile |
| `docker build -t node-test:1.0.0 .` | Construire l'image avec un nom et une version spécifique |

## VOLUMES
| Command | Description |
| :---: | :---: |
| `docker volume ls` | Lister tous les volumes |
| `docker volume create <volume>` | Créer un volume |
| `docker volume inspect <volume>` | Afficher les informations (format JSON) |
| `docker volume rm <volume>` | Détruire un volume |
| `docker volume ls --filter="dangling=true"` | Lister tous les volumes non référencés (dangling) |
| `docker volume prune` | Supprimer tous les volumes non référencés |
| `docker run --rm --volumes-from <container> -v $(pwd):/backup busybox tar cvfz /backup/backup.tar.gz <container-path>` | Sauvegarder un conteneur |
| `docker run --rm --volumes-from <container> -v $(pwd):/backup busybox sh -c "cd <container-path> && tar xvfz /backup/backup.tar.gz --strip 1"` | Restaurer un conteneur à partir d'une sauvegarde |
| `docker run -it -v "./app:/app//" node-test bash` | Lancer l'image en montant un volume |
| `docker run -it -v "./pwa:/pwa" node-test bash` | Lancer l'image avec un autre volume |
| `docker run -it -v "./app:/app" ubuntu bash` | Lancer l'image Ubuntu avec un volume |

## DIVERS
| Command + option | Description |
| :--------------: | :---------: |
| `uname -a` | Version Linux du container |
| `docker-compose up` |	Lancer les services définis dans docker-compose |

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
