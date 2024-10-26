# OPEN SSL

## INTRODUCTION
OpenSSL est un ensemble d'outils de chiffrement comprenant deux bibliothèques, libcrypto et libssl, fournissant respectivement une implémentation des algorithmes cryptographiques et du protocole de communication SSL/TLS, ainsi qu'une interface en ligne de commande.

## CERTIFICATE MANAGEMENT
| Commande | Description |
| :---: | :---: |
| `openssl req -new -key <key> -out <csr>` | Générer une nouvelle demande de signature de certificat |
| `openssl req -x509 -key <key> -in <csr> -out <cert>` | Générer un certificat auto-signé |
| `openssl x509 -in <cert> -text -noout` | Afficher les détails d'un certificat |
| `openssl x509 -in <cert> -pubkey -noout` | Extraire la clé publique d'un certificat |
| `openssl x509 -in <cert> -fingerprint -noout` | Afficher l'empreinte d'un certificat |

## KEY MANAGEMENT
| Commande | Description |
| :---: | :---: |
| `openssl genrsa -out <key> 2048` | Générer une nouvelle clé privée RSA |
| `openssl rsa -in <key> -pubout -out <pub_key>` | Extraire la clé publique d'une clé privée |
| `openssl rsa -in <key> -out <new_key>` | Convertir une clé privée dans un format différent |
| `openssl rand -hex 16` | Générer une chaîne hexadécimale aléatoire |

## CERTIFICATE SIGNING
| Commande | Description |
| :---: | :---: |
| `openssl ca -in <csr> -out <cert>` | Signer une demande de certificat |
| `openssl ca -config <config> -in <csr> -out <cert>` | Signer une demande de certificat avec une configuration personnalisée |
| `openssl verify -CAfile <ca> <cert>` | Vérifier un certificat contre un fichier CA |

## CERTIFICATE CONVERSION
| Commande | Description |
| :---: | :---: |
| `openssl pkcs12 -export -in <cert> -inkey <key> -out <file>` | Convertir un certificat et une clé au format PKCS#12 |
| `openssl pkcs12 -in <file> -out <cert> -nodes` | Extraire un certificat et une clé d'un fichier PKCS#12 |
| `openssl x509 -in <cert> -outform DER -out <file>` | Convertir un certificat au format DER |
| `openssl x509 -in <cert> -outform PEM -out <file>` | Convertir un certificat au format PEM |

## ENCRYPTION/DECRYPTION
| Commande | Description |
| :---: | :---: |
| `openssl enc -aes-256-cbc -salt -in <file> -out <encrypted_file>` | Chiffrer un fichier avec AES-256-CBC |
| `openssl enc -d -aes-256-cbc -in <file> -out <decrypted_file>` | Déchiffrer un fichier chiffré avec AES-256-CBC |
| `openssl dgst -sha256 FILE` | Calculer le hachage SHA-256 d'un fichier |
| `openssl dgst -md5 FILE` | Calculer le hachage MD5 d'un fichier |

## MISCELLANEOUS
| Commande | Description |
| :---: | :---: |
| `openssl version` | Afficher la version d'OpenSSL |
| `openssl s_client -connect <host>:<port>` | Se connecter à un serveur en utilisant SSL/TLS |
| `openssl s_server -accept <port> -cert <cert> -key <key>` | Démarrer un serveur SSL/TLS |
| `openssl speed` | Exécuter des tests de performance sur les algorithmes OpenSSL |
| `openssl ciphers -v` | Lister tous les chiffres disponibles |
| `openssl rand -base64 32` | Générer une chaîne base64 aléatoire |
| `openssl rand -base64 -out <file> 32` | Générer une chaîne base64 aléatoire et l'enregistrer dans un fichier |
| `openssl rand -out <file> 32` | Générer une chaîne binaire aléatoire et l'enregistrer dans un fichier |
| `openssl rand -hex 32` | Générer une chaîne hexadécimale aléatoire |

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
