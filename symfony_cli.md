# SYMFONY
## INTRODUCTION
Symfony est un framework MVC libre écrit en PHP. Développé par l'agence web française SensioLabs et son créateur Fabien Potencier, il est conçu pour faciliter la création d'applications web robustes et modulaires.
## INSTALLATION
### 1. Installer composer
[Composer](getcomposer.org.)
### 2. Vérifier version de composer
```shell
composer -v
```
### 2. Vérifier version PHP
```shell
php --version
```
- NOTE: 7.2 mini
### 3. Créer un projet Symfony
```shell
composer create-project symfony/website-skeleton nomProjet
```
### 4. Créer un serveur personnalisé pour lancer Symfony (en développement)
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
### 6. SGDB
Editer le fichier
```env
DATABASE_URL=mysql://root:root@127.0.0.1:3306/dbName
# DATABASE_URL="mysql://root:root@db:3306/dbName"
```
### Drop la base de données
```shell
php bin/console doctrine:database:drop --force
```
## MAPPING SGBD
## MIGRATION
```shell
php bin/console make:migration --no-interaction;
```
## COMMAND LINE INTERFACE
## FIXTURES
## LINTER
## TESTS
## DEMO INSTALLATION
## DIVERS
### FOS User Bundle
```shell
composer require friendsofsymfony/user-bundle:"version"
```
### Stop server
```shell
npx kill-port 8000
```
### Cache clear
```shell
php bin/console cache:clear
```





```shell

```



- Installer composer                                                    https://getcomposer.org/download/
- Vérifier version composer                                             composer -v
- Vérifier version php (7.2 mini)                                       php -version
- Créer projet (dans le repository github)                                composer create-project symfony/website-skeleton nomprojet
- Créer serveur personnalisé pour lancer symfony (en dev)               composer require server --dev
- Lancer application (localhost:8000)                                   php bin/console server:run

composer require friendsofsymfony/user-bundle"version"


- Fichier .env (ligne 27: DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name) et remplacer par =>
DATABASE_URL=mysql://root:@127.0.0.1:3306/blog

php bin/console doctrine:database:create
php bin/console make:entity  



composer require orm-fixtures --dev 
php bin/console make:fixtures (ArticleFixtures)


php bin/console doctrine:fixtures:load


composer require fzaninotto/faker


hp bin/console make:controller


php bin/console make:form

php bin/console make:migration
php bin/console doctrine:migrations:migrate

php bin/console make:controller



		LINTER/TEST
        		SYMFONY DEMO INSTALL
composer create-project symfony/symfony-demo:1.8.0 symfony-gitlab --no-interaction

composer create-project symfony/symfony-demo:1.8.0 symfony-local2 --no-interaction


symfony serve:start -d

1/ LINTER PHP-CS-FIXER
créer 2 dossiers tools>php-cs-fixer
composer require --working-dir=tools/php-cs-fixer friendsofphp/php-cs-fixer

Lancer analyse ( à la racine ) =>
php ./tools/php-cs-fixer/vendor/bin/php-cs-fixer fix --dry-run


2/ LINTER PHP STAN
composer require --dev phpstan/phpstan
composer require --dev phpstan/extension-installer
composer require --dev phpstan/phpstan-symfony

Lancer analyse ( à la racine ) =>
php vendor/bin/phpstan analyse src


3/ LINTER TWIG
Checker les fichiers HTML Twig en environnement de prod
php ./bin/console lint:twig templates --env=prod


4/ LINTER YAML
Checker les fichiers yaml
php ./bin/console lint:yaml config --parse-tags


5/ PHP UNIT
composer require --dev phpunit/phpunit
Lancer analyse =>
php ./bin/phpunit

Lancer tests unitaires PHP Unit =>
php ./bin/phpunit