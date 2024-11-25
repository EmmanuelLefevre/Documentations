# SSH

## SOMMAIRE
- [INTRODUCTION](#introduction)
- [WARNING](#warning)
- [COMMANDES](#commandes)
  - [Connection et authentification](#connection-et-authentification)
  - [Création des clés](#création-des-clés)
  - [Vérification et gestion des clés](#vérification-et-gestion-des-clés)
  - [Transfert de fichiers](#transfert-de-fichiers)
  - [Tunnel SSH et redirection de port](#tunnel-ssh-et-redirection-de-port)
  - [Débogage et options avancées](#débogage-et-options-avancées)
  - [Github](#github)
  - [Utilitaires](#utilitaires)

## INTRODUCTION
SSH (Secure Shell) est un protocole réseau qui permet de se connecter de manière sécurisée à un autre ordinateur sur un réseau, souvent via Internet. Il remplace les méthodes non sécurisées comme Telnet et fournit une couche de sécurité pour l'administration à distance, le transfert de fichiers, et l'exécution de commandes sur des systèmes distants.  
SSH fonctionne sur un modèle client-serveur et utilise le chiffrement pour sécuriser les communications entre les deux parties. Le serveur SSH est généralement configuré sur le port 22.  
Les fonctionnalités principales d’SSH incluent:  
- **Connexion à distance sécurisée:** Permet d'accéder à un système distant avec un chiffrement des données échangées.
- **Transfert sécurisé de fichiers:** Via les protocoles SCP et SFTP, SSH permet de transférer des fichiers en toute sécurité.
- **Tunnels SSH et redirections de port:** SSH permet de créer des tunnels pour sécuriser d'autres types de connexions.
- **Authentification par clés publiques:** Améliore la sécurité en remplaçant l’authentification par mot de passe par un système de clés publique/privée.  

## WARNING
Sur Windows, utilisez ces chemins:  
- C:/Users/<UserName>/.ssh/github_id_ed
- $env:USERPROFILE\.ssh\github_id_ed  

Sur Linux/Mac, utilisez:  
- ~/.ssh/github_id_ed

## COMMANDES
### Connection et authentification
| Command | Description |
| :---: | :---: |
| `ssh user@host` | Se connecter à un serveur SSH. |
| `ssh -p port user@host` | Se connecter à un serveur SSH sur un port spécifique. |
| `ssh -i /path/to/private_key user@host` | Se connecter avec une clé privée spécifique. |
| `ssh-copy-id user@host` | Copier clé publique vers un serveur (authentification par clé publique). |
| `ssh-add /path/to/private_key` | Ajouter clé privée à l'agent SSH (éviter de retaper le mdp). |
| `ssh-agent bash` puis `ssh-add path` | Démarrer agent SSH pour des connexions sans mdp. |
| `ssh -T user@hostname` | Tester connexion SSH avec service distant. |

### Création des clés
| Command | Description |
| :---: | :---: |
| `ssh-keygen -t rsa -b 4096` | Générer paire de clés SSH RSA de 4096 bits. |
| `ssh-keygen -t ed25519` | Générer une paire de clés SSH ED25519. |
| `ssh-keygen -t ecdsa -b 521` | Générer une paire de clés SSH ECDSA avec une clé de 521 bits. |

### Vérification et gestion des clés
| Command | Description |
| :---: | :---: |
| `eval "$(ssh-agent -s)"` | Démarrer l'agent SSH pour gérer les clés. |
| `ls ~/.ssh` | Vérifier la présence de clés SSH dans répertoire `.ssh`. |
| `ssh-add C:/Users/<UserName>/.ssh/github_id_ed` | Ajouter clé privée à l'agent SSH. |
| `ssh-add -l` | Lister les clés actuellement ajoutées à l'agent SSH. |
| `ssh-add -D` | Supprimer toutes les clés de l'agent SSH. |
| `ssh-add -d C:/Users/<UserName>/.ssh/github_id_ed` | Supprimer une clé spécifique de l'agent SSH. |
| `ssh -Q key` | Lister types de clés supportés par SSH (e.g., rsa, ecdsa). |

### Transfert de fichiers
| Command | Description |
| :---: | :---: |
| `scp file user@host:path` | Copier fichier local vers serveur distant. |
| `scp user@host:path file` | Copier fichier depuis un serveur distant vers ordinateur local. |
| `scp -r directory user@host:path` | Copier répertoire entier vers serveur distant. |
| `rsync -avz file user@host:path` | Synchroniser fichier ou répertoire local avec serveur distant. |
| `sftp user@host` | Se connecter en SFTP (Secure FTP) vers serveur SSH. |
| `sftp> get remote_file` | Télécharger fichier depuis serveur distant (depuis SFTP). |
| `sftp> put local_file` | Envoyer fichier vers serveur distant (depuis SFTP). |
| `sftp> mget *.ext` | Télécharger plusieurs fichiers correspondant à un modèle. |
| `sftp> mput *.ext` | Envoyer plusieurs fichiers correspondant à un modèle. |

### Tunnel SSH et redirection de port
| Command | Description |
| :---: | :---: |
| `ssh -L local_port:remote_host:remote_port user@host` | Créer tunnel SSH pour rediriger port local vers port distant. |
| `ssh -R remote_port:local_host:local_port user@host` | Rediriger port distant vers port local. |
| `ssh -D port user@host` | Créer proxy SOCKS avec SSH pour naviguer sur Internet via serveur distant. |
| `ssh -fNL local_port:remote_host:remote_port user@host` | Créer tunnel SSH en arrière-plan. |
| `ssh -L 8080:localhost:80 user@host` | Accéder à un serveur web distant localement sur port 8080. |

### Débogage et options avancées
| Command | Description |
| :---: | :---: |
| `ssh -v user@host` | Activer mode verbeux pour des informations détaillées sur la connexion. |
| `ssh -vvv user@host` | Activer mode verbeux maximal pour débogage approfondi. |
| `ssh -C user@host` | Activer compression pour la connexion SSH (connexions lentes). |
| `ssh -o StrictHostKeyChecking=no user@host` | Désactiver la vérification de la clé hôte. |
| `ssh -o "ExitOnForwardFailure yes" -L local_port:remote_host:remote_port user@host` | Terminer la connexion si le tunnel échoue. |

### Github
| Command | Description |
| :---: | :---: |
| `ssh -T git@github.com` | Tester connexion SSH avec GitHub. |
| `git remote set-url origin git@github.com:<UserName>/<RepoName>.git` | Configurer repository local pour utiliser SSH. |
| `git remote -v` | Vérifier la configuration du remote SSH. |
| **Ajouter clé dans GitHub** | **Profile > Settings > SSH and GPG keys** et ajouter clé publique. |

### Utilitaires
| Command | Description |
| :---: | :---: |
| `whoami` | Afficher nom d'utilisateur actuel. |
| `hostname` | Afficher nom de l'hôte. |
| `exit` | Quitter session SSH. |
| `logout` | Alternative à `exit` pour quitter session SSH. |
| `history` | Voir historique des commandes. |
| `eval $(ssh-agent -s)` | Lancer ou vérifier l'agent SSH avec le PID associé. |

***

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "[Dotfiles](https://github.com/EmmanuelLefevre/Dotfiles)" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
