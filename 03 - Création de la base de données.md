<h1 align="center">3 - Création de la base de données</h1>

Lien [ICI](https://www.youtube.com/watch?v=MhVAwrujifQ&list=PLBq3aRiVuwyzI0MT4LhvwqkVenz5pF_DM&)

---

<h2 align="center">Créer la base de donnée</h2>

Après avoir configuré le projet faites cette commande pour initialiser la base de donnée.

```shell
symfony console doctrine:database:create
```

Vous êtes censé avoir ce message en réponse

> Created database \`ecommerce_sf6\` for connection named default.

Si vous avez une erreur, il doit y avoir un problème avec le `DATABASE_URL` dans le `.env.local`, 

---

<h2 align="center">Créer un Utilisateur</h2>

Dans la console faites

```shell
symfony console make:user
```

Vous allez avoir plusieurs questions, je vais vous donner l'une après l'autre. (Si vide c'est que nous allons utiliser la valeur par défaut *celle entre crochet*, appuyez sur entrer)

- Users

- 

- 

- 

Nous allons ensuite ajouter un attribut à l'objet `Users` pour cela faites

```shell
symfony console make:entity
```

- Users

- lastname

- 

- 100

- 

- firstname

- 

- 100

- 

- address

- 

- 

- 

- zipcode

- 

- 5

- 

- city

- 

- 150

- 

- created_at

- 

- 

Après avoir fini, allez dans l'entité Users.php dans /src/Entity/Users.php

Allez sur la ligne au-dessus de `private $created_at`

```php
#[ORM\Column(type: 'datetime_immutable')]
```

Modifiez la ligne du dessus en ça

```php
#[ORM\Column(type: 'datetime_immutable', options: ['default' => 'CURRENT_TIMESTAMP'])]
```

<h2 align="center">Créer les Catégories</h2>

```shell
symfony console make:entity
```

- Categories

- name

- 

- 100

- 

- parent

- relation

- Categories

- ManyToOne

- 

- 

- 

-  

<h2 align="center">Créer les types de Coupon</h2>

```shell
symfony console make:entity
```

- CouponsTypes

- name

-  

- 50

-  

<h2 align="center">Créer les Coupons</h2>

```shell
symfony console make:entity
```

- Coupons

- code

-  

- 10

-  

- description

- text

-  

- discount

- integer

-  

- max_usage

- integer

-  

- validity

- datetime

-  

- is_valid

-  

-  

- created_at

-  

-  

- coupons_types

- relation

- CouponsTypes

- ManyToOne

- no

-  

-  

- yes

-  

Après avoir fini, allez dans l'entité `Coupons.php` dans `/src/Entity/Coupons.php`

Allez sur la ligne au-dessus de `private $code;`

```php
#[ORM\Column(type: ('string', length: 10)]
```

Et modifiez la en

```php
#[ORM\Column(type: ('string', length: 10, unique: true)]
```

Et comme pour la classe Users.php modifiez la ligne au-dessus de  `private $created_at`

<h2 align="center">Créer les Produits</h2>

```shell
symfony console make:entity
```

- Products

- name

-  

-  

-  

- description

- text

-  

- price

- integer

-  

- stock

- integer

-  

- created_at

-  

-  

- categories

- relation

- Categories

- ManyToOne

- no

-  

-  

-  

Après avoir fini, allez dans l'entité `Products.php` dans `/src/Entity/Coupons.php`

Comme pour la classe Users.php modifiez la ligne au-dessus de `private $created_at`

<h2 align="center">Créer les Images</h2>

```shell
symfony console make:entity
```

- Images

- name

-  

-  

-  

- products

- relation

- Products

- ManyToOne

- no

-  

-  

- yes

-  

<h2 align="center">Créer les Commandes</h2>

```shell
symfony console make:entity
```

- Orders

- reference

-  

-  20

-  

- created_at

-  

-  

- coupons

- relation

- Coupons

- ManyToOne

-  

-  

-  

- users

- relation

- Users

- ManyToOne

- no

-  

-  

-  

-  

Après avoir fini, allez dans l'entité `Orders.php` dans `/src/Entity/Orders.php`

Allez sur la ligne au-dessus de `private $reference;`

```php
#[ORM\Column(type: ('string', length: 20)]
```

Et modifiez la en

```php
#[ORM\Column(type: ('string', length: 20, unique: true)]
```

<h2 align="center">Créer les Details de commandes</h2>

```shell
symfony console make:entity
```

- OrdersDetails

- quantity

- integer

-  

- price

- integer

-  

- orders

- relation

- Orders

- ManyToOne

- no

-  

-  

- yes

- products

- relation

- Products

- ManyToOne

- no

-  

-  

-  

-  

Après avoir fini, allez dans l'entité `OrdersDetails.php` dans `/src/Entity/OrdersDetails.php`

Nous allons devoir modifier plusieurs choses

Premièrement supprimez ces lignes:

```php
#[ORM\Id]
#[ORM\GeneratedValue]
#[ORM\Column(type: 'integer')]
private $id;
```

Puis supprimez ces lignes

```php
public function getId: ?int
{
    return $this->id;
}
```

Nous allons ensuite ajouter ceci à deux endroits

```php
#[ORM\Id]
```

Avant

```php
#[ORM\ManyToOne(targetEntity: Orders::class, inversedBy: 'ordersDetails')]
```

```php
#[ORM\ManyToOne(targetEntity: Products::class, inversedBy: 'ordersDetails')]
```

---

<h2 align="center">Finalisation</h2>

Nous allons maintenant envoyer tous nos changements dans la base de donnée, pour ce faire nous allons exécuter ces commandes

```shell
symfony console make:migration
```

```shell
symfony console doctrine:migrations:migrate
```

Laissez la ligne vide quand vous avez une prompt



Et voilà, nous en avons finis avec la base de donnée, tout est en place pour la suite du tutoriel !
