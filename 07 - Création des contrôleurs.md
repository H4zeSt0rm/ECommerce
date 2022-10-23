<h1 align="center">7 - Création des contrôleurs</h1>

Lien [ICI](https://www.youtube.com/watch?v=X_mNHTGJb5M&list=PLBq3aRiVuwyzI0MT4LhvwqkVenz5pF_DM)

---

Nous allons faire d'autres contrôleurs pour le site

<h2 align="center">ProfileController</h2>

```shell
symfony console make:controller ProfileController
```

Allez ensuite dans `/src/Controller/ProfileController.php`

Modifiez le `#[Route('/profile', name: 'app_profile')]` en `#[Route('/', name: 'index')]`

Et ajoutez `#[Route('/profil', name: 'profile_')]` au dessus de `class ProfileController extends AbstractController`

Nous allons ajouter une nouvelle Route

```php
#[Route('/commandes', name: 'orders')]
public function orders(): Response
{
    return $this->render('profile/index.html.twig', [
        'controller_name' => 'Commandes',
    ]);
}
```

<h2 align="center">ProductsController</h2>

```shell
symfony console make:controller ProductsController
```

Et comme pour `ProfileController` nous allons mettre `#[Route('/products', name: 'products_')]` au dessus de `class ProductsController extends AbstractController` et modifions l'annotation au dessus d'`index()` en `#[Route('/', name: 'index')]` et pensez à supprimer les variables dans `render`

```php
, [
    'controller_name' => 'ProductsController',
]
```

Nous allons ensuite modifier `templates/products/index.html.twig`

```twig
{% extends 'base.html.twig' %}

{% block title %}Liste des produits{% endblock %}

{% block body %}

    <h1>Liste des produits</h1>

{% endblock %}
```

Nous allons ajouter une `Route` dans `ProductsController`

```php
#[Route('/{slug}', name: 'details')]
public function details(?Products $product): Response
{
    if (!isset($product)) return $this->redirectToRoute("products_index");
    return $this->render('products/details.html.twig', compact('product'));
}
```

Dupliquez `templates/products/index.html.twig` en `templates/products/details.html.twig` et modifiez le comme ceci

```twig
{% extends 'base.html.twig' %}

{% block title %}Détails de {{ product.name }}{% endblock %}

{% block body %}

    <h1>Détails de {{ product.name }}</h1>

{% endblock %}
```

<h2 align="center">Contrôleurs Administrateurs</h2>

```shell
symfony console make:controller Admin\UsersController
```

Comme pour les deux autres contrôleurs nous allons modifier les annotations.

Mettez `#[Route('/admin/utilisateurs', name: 'admin_users_')]` au dessus de `class UsersController extends AbstractController` et faites en sorte que votre `index()` ressemble à ça

```php
#[Route('/', name: 'index')]
public function index(): Response
{
    return $this->render('admin/users/index.html.twig');
}
```

Et modifiez le fichier `templates/admin/users/index.html.twig` en

```twig
{% extends 'base.html.twig' %}

{% block title %}Administration des utilisateurs{% endblock %}

{% block body %}
    <h1>Administration des utilisateurs</h1>
{% endblock %}
```


