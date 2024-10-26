# SYMFONY CLI

## INTRODUCTION
Symfony est un ensemble de composants PHP ainsi qu'un framework MVC libre écrit en PHP. Il fournit des fonctionnalités modulables et adaptables qui permettent de faciliter et d’accélérer le développement d'un site web de manière robuste et modulaire. Il est développé par l'agence web française SensioLabs et son créateur Fabien Potencier.

## CLI
| Command | Description |
| :--------------: | :---------: |
| `php bin/console list`| Toutes les commandes |
| `php bin/console help` | Help |
| `php bin/console make:entity` | Entity |
| `php bin/console make:controller` | Controller |
| `php bin/console make:form` | Form |
| `php bin/console make:registration-form` | Registration form |
| `php bin/console make:user` | User |
| `php bin/console make:crud` | CRUD |
| `php bin/console make:reset-password` | Reset password |
| `php bin/console security:hash-password` | Hash password |
| `php bin/console make:subscriber` | Subscriber |
| `php bin/console make:validator` | Validator |

## DEBUG
| Command | Description |
| :--------------: | :---------: |
| `php bin/console debug:autowiring` | Autowiring |
| `php bin/console debug:firewall` | Firewall |
| `php bin/console debug:config` | Configuration |
| `php bin/console debug:config profiler` | Profiler |
| `php bin/console debug:router` | Debug router |
| `php bin/console debug:container` | Container |
| `php bin/console debug:twig` | Twig |
| `php bin/console debug:validator` | Validator |
| `php bin/console debug:event-dispatcher` | Listener priority |

## SGBD
| Command | Description |
| :--------------: | :---------: |
| `php bin/console doctrine:database:drop --force`| Drop bdd |

## SECRETS
| Command | Description |
| :--------------: | :---------: |
| `php bin/console secrets:generate-keys` | New secret |
| `php bin/console secrets:list` | List of secrets |
| `php bin/console secrets:remove` | Delete secrets |
| `php bin/console secrets:set` | Set secret in vault |
| `php bin/console secrets:decrypt-to-local` | Decrypt all secrets and stores them in the local vault |
| `php bin/console secrets:encrypt-from-local` | Encrypt all local secrets to the vault |

## CACHE
| Command | Description |
| :--------------: | :---------: |
| `php bin/console cache:clear`| Clear cache |
| `php bin/console cache:pool:clear`| Clear pool cache |

## LEXIK JWT
| Command | Description |
| :--------------: | :---------: |
| `php bin/console lexik:jwt:check-config`| Checks JWT configuration |
| `php bin/console lexik:jwt:generate-keypair`| Générer clés privé/publique |
| `php bin/console lexik:jwt:generate-token`| Générer un token JWT |

## DEMO INSTALLATION
| Command | Description |
| :--------------: | :---------: |
| `composer create-project symfony/symfony-demo:1.8.0 symfony-local2 --no-interaction`| Installer version de démo depuis github |
| `composer create-project symfony/symfony-demo:1.8.0 symfony-gitlab --no-interaction`| Installer version de démo en local |

## GESTION DES ASSETS
| Command | Description |
| :--------------: | :---------: |
| `composer require symfony/webpack-encore-bundle`| Installer Webpack Encore |
| `npm run dev`| Compiler les assets en dev |
| `npm run build`| Compiler les assets en prod |

## MISCELLANEOUS
| Command | Description |
| :--------------: | :---------: |
| `npx kill-port 8000`| Stoper server local |
| `php bin/console config:dump-reference`| Afficher la configuration par défaut d'un bundle spécifique |
| `composer require friendsofsymfony/user-bundle:"version"`| Installer FosUserBundle à la version spécifique |

⭐⭐⭐ I hope you enjoy it, if so don't hesitate to leave a like on this repository and on the "Settings" one (click on the "Star" button at the top right of the repository page). Thanks 🤗
