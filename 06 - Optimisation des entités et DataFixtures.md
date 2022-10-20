<h1 align="center">6 - Optimisation des entités et DataFixtures</h1>

Lien [ICI](https://www.youtube.com/watch?v=JVVeBiewhNg&list=PLBq3aRiVuwyzI0MT4LhvwqkVenz5pF_DM)

---

<h2 align="center">Création de traits</h2>

Les traits permettent à votre code de réutiliser des attributs et des fonctions.

Tout d'abord nous allons faire un dossier dans le répertoire `/src/Entity/`, le dossier s'appellera `Trait`.

<h3 align="center">Le trait CreatedAt</h3>

Le premier trait que nous allons faire c'est `CreatedAtTrait.php`, le contenu du fichier ressemblera à ça

```php
<?php

namespace App\Entity\Trait;

use Doctrine\ORM\Mapping as ORM;


trait CreatedAtTrait
{
    #[ORM\Column(type:'datetime_immutable', options: ['default' => 'CURRENT_TIMESTAMP'])]
    private $created_at;

    public function getCreatedAt() : ?\DateTimeImmutable 
    {
        return $this->created_at;
    }

    public function setCreatedAt(\DateTimeImmutable $created_at) : self
    {
        $this->created_at = $created_at;
        return $this;
    }

}
```

Vous allez devoir retirer la déclaration de l'attribut dans les entités (Coupons; Orders; Products; Users) et les deux fonctions ajoutées dans le trait et à la place juste après la déclaration de la classe dans chaque fichier ajoutés 

```php
class ...
{
    use CreatedAtTrait;
}
```

*Si vous n'avez pas IntelliPhense pensez à ajouter la dépendance dans chaque fichier*

Juste après namespace ajoutez `use App\Entity\Trait\CreatedAtTrait`



Il faut aussi ajouter dans les constructeurs des entités mentionnées au-dessus cette ligne

```php
$this->created_at = new \DateTimeImmutable();
```

<h3 align="center">Le trait Slug</h3>

Faites un nouveau trait, celui-là sera nommé `SlugTrait`.

```php
<?php

namespace App\Entity\Trait;

use Doctrine\ORM\Mapping as ORM;


trait SlugTrait
{
    #[ORM\Column(type:'string', length:255 )]
    private $slug;

    public function getSlug() : ?string
    {
        return $this->slug;
    }

    public function setSlug(string $slug) : self
    {
        $this->slug = $slug;
        return $this;
    }

}
```

Nous allons ajouter à `Categories` et `Products`

```php
use SlugTrait;
```

et faire un 

```shell
symfony console m:migr
symfony console d:m:m
```

pour ajouter slug au entités de la base de donnnée

---

<h2 align="center">DataFixtures</h2>

Nous allons maintenant attaquer les DataFixtures, cela nous permettre de générer un jeu d'essai à partir de code.

Pour commencer faites cette commande

```shell
composer require --dev orm-fixtures
```

Après la commande vous allez avoir un nouveau dossier dans l'application

`/src/DataFixtures` avec un fichier `AppFixtures.php`

Nous allons aussi avoir besoin de `fakerphp` pour l'installer faites

```shell
composer require fakerphp/faker
```

Ensuite nous allons faire le DataFixture pour les Categories

```shell
symfony console make:fixtures CategoriesFixtures
```

Nous pouvons maintenant commencer à générer nos données, dans un premier temps nous allons ajouter un constructeur et une variable à `CategoriesFixtures.php`

```php
private $counter = 1;

public function __construct(private SluggerInterface $slugger){}
```

Pour nous faciliter la tâche et pour ne pas remettre plusieurs fois les mêmes lignes de code nous allons ajouter une autre fonction

```php
public function createCategory(string $name, Categories $parent=null, ObjectManager $manager)
{
    $category = new Categories();
    $category->setName($name);
    $category->setSlug($this->slugger->slug($name)->lower());
    $category->setParent($parent);
    $manager->persist($category);

    $this->addReference('cat-'.$this->counter, $category);
    $this->counter++;

    return $category;    
}
```

Ensuite dans la fonction load nous pouvons écrire ceci

```php
$parent = $this->createCategory('Informatique', null, $manager);
$this->createCategory('Ordinateurs portables', $parent, $manager);
$this->createCategory('Ecrans', $parent, $manager);
$this->createCategory('Souris', $parent, $manager);


$parent = $this->createCategory('Mode', null, $manager);
$this->createCategory('Homme', $parent, $manager);
$this->createCategory('Femme', $parent, $manager);
$this->createCategory('Enfant', $parent, $manager);
```

Nous allons devoir aussi régler un problème dans l'entité `Categories`, nous allons devoir ajouter une annotation au-dessus de `private $parent;`

```php
#[ORM\JoinColumn(onDelete: 'CASCADE')]
```

Vous pouvez maintenant générer les fixtures 

```shell
symfony console doctrine:fixtures:load --no-interaction
```

Si vous regardez votre base de donnée vous allez voir des modifications dans la table `Categories`.

Nous allons maintenant faire d'autres Fixtures

```shell
symfony console make:fixtures UsersFixtures
symfony console make:fixtures ProductsFixtures
symfony console make:fixtures ImagesFixtures
```

Nous allons commencer par configurer UsersFixtures

D'abords ajouter un constructeur

```php
public function __construct(private UserPasswordHasherInterface $passwordEncoder, private SluggerInterface $slugger){}
```

Et maintenant la fonction load, *si vous avez une erreur il sur Faker\Factory ajoutez `use Faker;` en haut après `namespace`*

```php
$admin = new Users();
$admin->setEmail('admin@demo.fr');
$admin->setLastname('Gambier');
$admin->setFirstname('Benoit');
$admin->setAddress('12 rue du port');
$admin->setZipcode('75001');
$admin->setCity('Paris');
$admin->setPassword(
    $this->passwordEncoder->hashPassword($admin, 'admin')
);
$admin->setRoles(['ROLE_ADMIN']);

$manager->persist($admin);

#Nous allons maintenant utiliser Faker
$faker = Faker\Factory::create('fr_FR');


for ($usr = 1; $usr <= 5; $usr++) {
    $user = new Users();
    $user->setEmail($faker->email);
    $user->setLastname($faker->lastName);
    $user->setFirstname($faker->firstName);
    $user->setAddress($faker->streetAddress);
    $user->setZipcode(str_replace(' ','',$faker->postcode));
    $user->setCity($faker->city);
    $user->setPassword(
        $this->passwordHasher->hashPassword($user,'secret')
    );    
    $manager->persist($user);
}
$manager->flush();
```

Nous allons ensuite faire Products, le constructeur seras

```php
public function __construct(private SluggerInterface $slugger) {}
```

Et load

```php
$faker = Faker\Factory::create('fr_FR');

for ($prod = 1; $prod <= 10; $prod++){
    $product = new Products();
    $product->setName($faker->text(5));
    $product->setDescription($faker->text());
    $product->setSlug(
        $this->slugger->slug($product->getName())->lower()  
    );
    $product->setPrice($faker->numberBetween(900, 150000));
    $product->setStock($faker->numberBetween(0,10));
    $category = $this->getReference('cat-'.rand(1,8));
    $product->setCategories($category);
    $manager->persist($product);
    $this->addReference('prod-'.$prod,$product);
}

$manager->flush();
```

Et pour finir nous allons configurer `ImagesFixtures.php`, nous n'avons pas besoin d'un constructeur custom, juste de modifier le load

```php
$faker = Faker\Factory::create('fr_FR');

for($img = 1; $img <= 100; $img++) {
    $image = new Images();
    $image->setName($faker->image(null,640,480));
    $product = $this->getReference('prod-'.rand(1,10));
    $image->setProducts($product);

    $manager->persist($image);
}
$manager->flush();
```

Si nous faisions un migrate maintenant nous aurions une erreur, car `DataFixtures` essaieras de faire les Images avant les Produits, par conséquent les références de produits n'existent pas encore. Pour régler ce problème nous allons implémenter la classe `DependentFixtureInterface`.

Pour utiliser cette implémentation nous allons devoir ajouter une fonction

```php
public function getDependencies() : array {
    return [
        ProductsFixtures::class
    ];
}
```



Vous pouvez maintenant faire

```shell
symfony console doctrine:schema:update --force
symfony console doctrine:fixtures:load --no-interaction
```

Votre base de donnée devrait être mise en place avec votre jeu de donnée.
