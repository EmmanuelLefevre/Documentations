# SYMFONY
## INTRODUCTION
Symfony est un framework MVC libre écrit en PHP. Développé par l'agence web française SensioLabs et son créateur Fabien Potencier, il est conçu pour faciliter la création d'applications web robustes et modulaires.
## INSTALLATION
### 1. Installer composer
[Composer](https://getcomposer.org/download)
### 2. Vérifier version de composer
```shell
composer -v
```
### 2. Vérifier version PHP
```shell
php --version
```
- NOTE: 7.2 mini
### 3. Créer projet Symfony
```shell
composer create-project symfony/website-skeleton nomProjet
```
### 4. Créer serveur personnalisé pour lancer Symfony (dev)
```shell
composer require server --dev
```
### 5. Lancer l'application (localhost:8000)
```shell
php bin/console server:run
```
```shell
php -S 127.0.0.1:8000 -t ./public
```
- NOTE: La seconde commande ne nécessite pas l'installation du serveur personnalisé!
### 6. Configurer le SGDB
Editer le fichier .env.local
```env
###> doctrine/doctrine-bundle ###
DATABASE_URL=mysql://root:root@127.0.0.1:3306/dbName
# DATABASE_URL="mysql://root:root@db:3306/dbName"
# DATABASE_URL="postgresql://symfony:ChangeMe@127.0.0.1:5432/app?serverVersion=13&charset=utf8"
# DATABASE_URL="sqlite:///%kernel.project_dir%/var/data.db"
###< doctrine/doctrine-bundle ###
```
### 7. Créer le SGDB
Editer le fichier .env.local
```bash
php bin/console doctrine:database:create
```
### 8. CORS
#### Installer le bundle Nelmio
```shell
composer require nelmio/cors-bundle
```
#### Configurer le bundle Nelmio
Editer le fichier .env.local
```env
###> nelmio/cors-bundle ###
CORS_ALLOW_ORIGIN='^https?://(localhost|127\.0\.0\.1)(:[0-9]+)?$'
# CORS_ALLOW_ORIGIN='^http:\/\/localhost:4200$|^https:\/\/mail.proton.me$'
# CORS_ALLOW_ORIGIN='^http:\/\/localhost:4200$'
###< nelmio/cors-bundle ###
```
### 9. JWT
#### Installer le bundle LexikJWTAuthenticationBundle
```shell
composer require lexik/jwt-authentication-bundle
```
#### Créer répertoire config/jwt si inexistant
```shell
mkdir -p config/jwt
```
#### Générer clé privé
```shell
openssl genpkey -out config/jwt/private.pem -algorithm RSA -pkeyopt rsa_keygen_bits:4096
```
#### Générer clé publique
```shell
openssl rsa -pubout -in config/jwt/private.pem -out config/jwt/public.pem
```
#### Sécuriser la clé privée
```shell
chmod 600 config/jwt/private.pem
```
#### Configurer l'environnement
Editer le fichier .env.local
```env
###> lexik/jwt-authentication-bundle ###
JWT_SECRET_KEY=%kernel.project_dir%/config/jwt/private.pem
JWT_PUBLIC_KEY=%kernel.project_dir%/config/jwt/public.pem
JWT_TTL=172800
###< lexik/jwt-authentication-bundle ###
```
#### Configurer le bundle dans Symfony
- Ajouter dans config/packages/lexik_jwt_authentication.yaml
```yaml
lexik_jwt_authentication:
    secret_key: '%env(resolve:JWT_SECRET_KEY)%'
    public_key: '%env(resolve:JWT_PUBLIC_KEY)%'
    token_ttl: "%env(JWT_TTL)%"
    user_identity_field: email
```
#### Configurer le firewall pour l'authentification JWT
- Modifier la configuration de sécurité dans le fichier config/packages/security.yaml pour ajouter un firewall et permettre l'authentification JWT
```yaml
security:
    firewalls:
        login:
            pattern:  ^/api/login
            stateless: true
            anonymous: true
            json_login:
                check_path:               /api/login
                username_path:             email
                password_path:             password
                success_handler:           lexik_jwt_authentication.handler.authentication_success
                failure_handler:           lexik_jwt_authentication.handler.authentication_failure

        api:
            pattern:   ^/api
            stateless: true
            provider:  users_in_memory
            guard:
                authenticators:
                    - lexik_jwt_authentication.jwt_token_authenticator

    access_control:
        - { path: ^/api/login, roles: IS_AUTHENTICATED_ANONYMOUSLY }
        - { path: ^/api,       roles: IS_AUTHENTICATED_FULLY }
```
#### Tests JWT
##### Génération JWT
Pour générer un token JWT (lors de la connexion de l'utilisateur) faire une requête POST à l'URL /api/login avec les informations de connexion
```shell
curl -X POST -H "Content-Type: application/json" http://localhost/api/login -d '{"email": "user@example.com", "password": "password"}'
```
#####  Utiliser un token pour les requêtes API sécurisées
Il suffit d'envoyer le token dans l'en-tête Authorization de chaque requête, avec le préfixe Bearer.
```shell
curl -X GET -H "Authorization: Bearer <token>" http://localhost/api/secure-route
```
### 10. Créer une entité
```shell
php bin/console make:entity
```
### 11. Générer le fichier de migration
#### Synchroniser la base de données avec les changements apportés à l'entité
```shell
php bin/console make:migration
```
#### Exécuter la migration pour mettre à jour la bdd
```shell
php bin/console doctrine:migrations:migrate --no-interaction
```
#### Validation du mapping
```shell
php bin/console doctrine:schema:validate
```
### 12. Générer la fixture si existante
```shell
php bin/console make:fixtures
```
### 13. Lancer le serveur
```shell
symfony serve:start -d
```
## SGDB
### Drop la base de données
```shell
php bin/console doctrine:database:drop --force
```
## COMMAND LINE INTERFACE
### Toutes les commandes
```shell
php bin/console list
```
### Help
```shell
php bin/console help
```
### Entity
```shell
php bin/console make:entity
```
### Controller
```shell
php bin/console make:controller
```
### Form
```shell
php bin/console make:form
```
### Registration form
```shell
php bin/console make:registration-form
```
### Entity
```shell
php bin/console make:entity
```
### User
```shell
php bin/console make:user
```
### CRUD
```shell
php bin/console make:crud
```
### Password
#### Reset password
```shell
php bin/console make:reset-password
```
#### Hash password
```shell
php bin/console security:hash-password
```
### Subscriber
```shell
php bin/console make:subscriber
```
### Validator
```shell
php bin/console make:validator
```
## FIXTURES
### Faker
```shell
composer require fzaninotto/faker --dev
```
```shell
php bin/console doctrine:fixtures:load
```
### ORM fixture
```shell
composer require orm-fixtures --dev
```
```shell
php bin/console make:fixtures
```
## LINTER
### PHP-CS-Fixer
```shell
mkdir -p tools/php-cs-fixer
```
```shell
composer require --working-dir=tools/php-cs-fixer friendsofphp/php-cs-fixer
```
Lancer analyse
```shell
php ./tools/php-cs-fixer/vendor/bin/php-cs-fixer fix --dry-run
```
### PHPStan
```shell
composer require --dev phpstan/phpstan
```
```shell
composer require --dev phpstan/extension-installer
```
```shell
composer require --dev phpstan/phpstan-symfony
```
Lancer analyse
```shell
php vendor/bin/phpstan analyse src
```
### Linter Twig
```shell
php ./bin/console lint:twig templates --env=dev
```
### Linter YAML
```shell
php ./bin/console lint:yaml config --parse-tags
```
## TESTS
### PHPUnit
Installer PHPUnit
```shell
composer require --dev phpunit/phpunit
```
Lancer l'analyse
```shell
php ./bin/phpunit
```
Lancer les tests unitaires
```shell
php ./bin/phpunit
```
## DEBUG
### Autowiring
```shell
php bin/console debug:autowiring
```
### Firewall
```shell
php bin/console debug:firewall
```
### Configuration
```shell
php bin/console debug:config
```
### Profiler
```shell
php bin/console debug:config profiler
```
### Debug router
```shell
php bin/console debug:router
```
### Container
```shell
php bin/console debug:container
```
### Twig
```shell
php bin/console debug:twig
```
### Validator
```shell
php bin/console debug:validator
```
### Listener priority
```shell
php bin/console debug:event-dispatcher
```
## SECRETS
### New secret
```shell
php bin/console secrets:generate-keys
```
### List of secrets
```shell
php bin/console secrets:list
```
### Delete secrets
```shell
php bin/console secrets:remove
```
### Set secret in vault
```shell
php bin/console secrets:set
```
### Decrypt all secrets and stores them in the local vault
```shell
php bin/console secrets:decrypt-to-local
```
### Encrypt all local secrets to the vault
```shell
php bin/console secrets:encrypt-from-local
```
## GESTION DES ASSETS
### 1. Installer Webpack Encore
```shell
composer require symfony/webpack-encore-bundle
```
### 2. Compiler les assets
- dev
```shell
npm run dev
```
- prod
```shell
npm run build
```
## CACHE
### Clear cache
```shell
php bin/console cache:clear
```
### Clear cache pools
```shell
php bin/console cache:pool:clear
```
## LEXIK JWT
### Checks JWT configuration
```shell
php bin/console lexik:jwt:check-config
```
### Générer clés privé/publique
```shell
php bin/console lexik:jwt:generate-keypair
```
### Générer un token JWT
```shell
php bin/console lexik:jwt:generate-token
```
## DIVERS
### FOS User Bundle
```shell
composer require friendsofsymfony/user-bundle:"version"
```
### Stop server
```shell
npx kill-port 8000
```
### Configuration
```shell
php bin/console config:dump-reference
```
## DEMO INSTALLATION
```shell
composer create-project symfony/symfony-demo:1.8.0 symfony-gitlab --no-interaction
```
```shell
composer create-project symfony/symfony-demo:1.8.0 symfony-local2 --no-interaction
```
