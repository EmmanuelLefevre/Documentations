# SYMFONY
## INTRODUCTION
Symfony est un un framework MVC libre écrit en PHP.
## COMMANDES UTILES












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
(pas de tests unitaires sur repo => couche DAO, getter/setter, controller)




------INSTALLATION------

- Installer composer                                                    https://getcomposer.org/download/
- Vérifier version composer                                             composer -v
- Vérifier version php (7.2 mini)                                       php -version
- Créer projet (dans le repository github)                                composer create-project symfony/website-skeleton nomprojet
- Créer serveur personnalisé pour lancer symfony (en dev)               composer require server --dev                !!! Se placer dans le dossier du projet !!!
- Lancer application (localhost:8000)                                   php bin/console server:run

- Installer FOS Bundle                                                  composer require friendsofsymfony/user-bundle"version"
(vérifier l'installation FOSUserBundle dans le dossier "vendor" du projet) puis l'activer dans le kernel app/ressources/appkernel.php 
en ajoutant la ligne new FOS\UserBundle\FOSUserBundle(),         
et ce à la fin de la public function registerBundles() et dans la variable $bundles


------Doctrine------

- Fichier .env (ligne 27: DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name) et remplacer par =>
DATABASE_URL=mysql://root:@127.0.0.1:3306/blog
- Créer bdd                                                             php bin/console doctrine:database:create
- Créer entité (table bdd)                                              php bin/console make:entity  

=> puis renseigner nom de la table et ensuite renseigner le nom des champs (en camelCase si pls mots) de la bdd ainsi que leur type
astuce sur le field type taper "? + entrée" pour afficher dans la console tout les types possibles
=> pour finir il faut migrer la table                                   php bin/console make:migration
                                                                        php bin/console doctrine:migrations:migrate


------Fixtures------

- Installer composant de création de fixtures                           composer require orm-fixtures --dev 
                                                                        php bin/console make:fixtures (ArticleFixtures)
- Puis aller dans le fichier ArticlesFixtures.php et instancier la classe dans la public function ( ex: $article = new Article(); )
ensuite renseigner le path (path=namespace haut du fichier App\Entity\Article.php) de la classe avec use (=require once) =>   
use App\Entity\Article;

- Renseigner l'article dans la public function à la suite de l'instanciation
$article->setTitle("Titre de l'article n°$i")
                    ->setContent("<p>Contenu de l'article n°$i</p>")
                    ->setImage("http://placehold.it/350x150")
                    ->setCreatedAt(new \DateTime());
                    
            $manager->persist($article);
        }

    $manager->flush();
}

- Charger fixtures dans la bdd                                php bin/console doctrine:fixtures:load
Attention: "yes" pour valider l'opération va purger la bdd actuelle

- Faker installation                                composer require fzaninotto/faker


------Repository------

Le repository permet de sélectionner des données dans une table..

- Avoir accès au repository au sein du controller pour sélectionner des données dans ma table (aller dans le controller 
de la classe en question et copier code ci-dessous dans la public function index et au dessus du "return $this->render....")
$repo = $this->getDoctrine()->getRepository(Article::class);
Ne pas oublier de déclarer le path dans le namespace en haut du controller => use App\Entity\Article;


------Créer Controller------

- Créer (puis renseigner le nom du controller: BlogController)          php bin/console make:controller


------Injection Dépendances (service container)------

Notion de dépendance: quand une classe/fonction a besoin de quelque chose pour fonctionner!
!!!PHP Type Hinting!!!: donner des indices sur le type de paramètre attendu!

Déclarer en attribut l'instanciation de la classe ArticleRepository dans la fonction publique index
=> public function index(ArticleRepository $repo)

!!! Ne pas oublier de déclarer le namespace en haut de fichier
=> use App\Repository\ArticleRepository;

Nous pouvons donc effacer la ligne suivante
=> $repo = $this->getDoctrine()->getRepository(Article::class);
car on demandait à doctrine de nous donner un repository alors que nous y avons mtnt accès grâce au système d'injection de 
dépendance, en effet Symfony comprend que quand il appelle index il doit passer à cette fonction un repository dans la variable $repo

Au même titre nous pouvons faire de même pour la fonction "show"
=> public function show(ArticleRepository $repo, $id)
et supprimer 
=> $repo = $this->getDoctrine()->getRepository(Article::class);


!!!Param Converter!!! => Convertir un param de la requête en objet:
=> public function show(Article $article)

Nous pouvons donc supprimer cette ligne inutile:
=> $article = $repo->find($id);

Symfony comprends que la fonction "show" a besoin de faire passer un article et dans la route nous disposons d'un identifiant,
donc il va chercher l'article qui a l'identifiant demandé (peut fonctionner avec d'autres champs que Id: ex titre,contenu...)


------FORMULAIRES------

- Création                  php bin/console make:form
puis renseigner le nom du formulaire (doit se finir par Type par convention)
puis définir si le formulaire se base sur une entité (ici Article)
visible dans le dossier src/Form

- Validation: 
importer namespace              use Symfony\Component\Validator\Constraints as Assert;
ex: @Assert\NotBlank            -> dans le fichier de l'entité en question


------THEME BOOTSTRAP------

Importer thème bootstrap => Copier dans config/packages/twig.yaml =>
form_themes: ['bootstrap_4_layout.html.twig']

Puis là on l'on veut utiliser notre formulaire (fichier .html.twig)
copier {% form_theme formArticle 'bootstrap_4_layout.html.twig' %} en en-tête du fichier de la vue html.



                        ---------------------
                        ------Namespace------
                        ---------------------

- Installer l'extension vscode "php namespace resolver"
Générer namespace           CTRL+ALT+i

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Bridge\Doctrine\Form\Type\EntityType;
use Symfony\Component\Routing\Annotation\Route;
use Symfony\Component\HttpFoundation\Request;
use Symfony\Component\Form\Extension\Core\Type\TextType;
use Symfony\Component\Form\Extension\Core\Type\TextareaType;
use Symfony\Component\Form\Extension\Core\Type\SubmitType;
use Symfony\Component\Form\AbstractType;
use Symfony\Component\Form\FormBuilderInterface;
use Symfony\Component\Form\Extension\Core\Type\PasswordType;
use Symfony\Component\OptionsResolver\OptionsResolver;
use Symfony\Component\Validator\Constraints as Assert;

use App\Entity\Article;
use App\Repository\ArticleRepository;
use App\Form\ArticleType;

use Doctrine\Common\Persistence\ObjectManager;
use Doctrine\ORM\Mapping as ORM;


                        -----------------------------
                        ------ENTITES/RELATION-------
                        -----------------------------




                        -----------------------------
                        ------AUTHENTIFICATION-------
                        -----------------------------


Installer composant de sécurité (Security Component) 

Firewalls -> -Quelle partie de l'appli on protège?
             -Comment on protège?
Providers -> -Ou sont les données des utilisateurs (annuaire,bdd,fichiers...)?
             -Comment reconnaitre les utilisateurs?
Encoders -> -Comment créer des hash, quel algorythme?
            -Possibilité d'encodeurs différents en fonction des entités

Ouvrir le fichier app/config/packages/security.yaml

!!!Note importante!!! => Ne pas faire de tabulation pour indenter un fichier .yaml, n'utiliser que des espaces.


1/ Créer une entité UserBundle                      php bin/console make:entity User
new property name = email de type string de 255 caractères, champ non null.
new property name = username de type string de 255 caractères, champ non null.
new property name = password de type string de 255 caractères, champ non null.

2/ Migrer la nouvelle table                         php bin/console make:migration
                                        puis        php bin/console doctrine:migrations:migrate

3/ Terminal                                         php bin/console make:form RegistrationType
name of entity = User

4/ Ajouter un champ dans le builder de la fonction publique buildForm fichier app/src/Form/RegistrationType
->add('confirm_password')

5/ Ajouter la ligne suivante                        public $confirm_password;
dans le fichier app/src/Entity/User.php et entre la méthode privée password et la fonction publique getId()

6/ Créer controller                                 php bin/console make:controller
name = SecurityController

7/ Se rendre dans le fichier app/src/Controller/SecurityController  et supprimer tout le code dans la classe
et le remplacer par  

   /**
     * @Route("/inscription", name="security_registration")
     */
    public function registration() {
        $user = new User();

        $form = $this->createForm(RegistrationType::class, $user);

        return $this->render('security/registratin.html.twig', [
            'form' => $form->cretaeView()
        ]);
    }

8/ Puis créer registration.html.twig dans le dossier app/src/templates/security
Et créer la vue du formulaire....

9/ Se rendre dans le fichier app/src/Form/RegistrationType et ajouter la classe PasswordType
->add('password', PasswordType::class)

10/ Activer des "Contraintes" (confirmation du password), se rendre dans le fichier app/src/Entity/User.php et
importer le namespace  use Symfony\Component\Validator\Constraints as Assert;


11/ Test github!!!!!!!!!!!!!!!!!!!!!!!!
11/ Test github!!!!!!!!!!!!!!!!!!!!!!!!
11/ Test github!!!!!!!!!!!!!!!!!!!!!!!!







