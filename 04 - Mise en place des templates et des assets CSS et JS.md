<h1 align="center">4 - Mise en place des templates et des assets CSS et JS</h1>

</div>

Lien [ICI](https://www.youtube.com/watch?v=aqw1bgitDcE&list=PLBq3aRiVuwyzI0MT4LhvwqkVenz5pF_DM)

---

Pour suivre le tutoriel, si votre projet Symfony a "webpack\_encore_bundle" d'installé, faites

```shell
composer remove webpack
```

Car nous n'allons pas avoir besoin de ce dernier.

---

<h2 align="center">Création du contrôleur principal</h2>

Pour faire notre contrôleur d'accueil nous devons faire

```shell
symfony console make:controlleur MainController
```

Vous avez maintenant un fichier `/src/Controller/MainController.php` et `/templates/main/index.html.twig`

Si vous voulez que la page d'accueil soit liée à votre contrôleur il faut modifier l'attribut `Route` de la fonction `index` dans `MainController.php`, remplacez

```php
#[Route('/main', name:'main')]
```

```php
#[Route('/', name:'main')]
```

Si vous lancez votre site avec

```shell
symfony serve -d # -d permet de lancer le site et de garder contrôle du shell
```

et que vous vous rendez dessus vous verrez une page de Bienvenue

![](.\assets\04%20-%20Mise%20en%20place%20des%20templates%20et%20des%20assets%20CSS%20et%20JS\main_controller.png)

Si vous retirez 

```php
, [
    'controller_name' => 'MainController'
]
```

de `MainController.php`, vous allez avoir une erreur, votre contrôleur est lié au fichier `index.html.twig`, vous allez donc devoir retirer la ligne qui utilise la variable que vous avez supprimé. Mais pour le bien de ce tutoriel nous allons juste vider tout ce qui se trouve entre les deux blocks body

```twig
{% block body %}
...
{% endblock %}
```

Et à la place écrire quelque chose comme 

```html
<p>Accueil</p>
```

La structure de toutes les pages de votre site se trouve dans le fichier `/templates/base.html.twig`

Vous pouvez directement supprimer le commentaire 

```twig
{# Run composer ... #}
```

Et vider les balises `stylesheets` et `javascripts` car nous n'allons pas utiliser `Webpack-encore-bundle`

Ajoutez avant le block stylesheets cette ligne important pour le niveau responsive de votre site

```twig
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

---

<h2 align="center">Création des assets</h2>

Pour mettre en place les assets allez dans le dossier `public` dans lequel vous allez faire un dossier `assets` et dans lequel vous allez faire deux dossiers `js` et `css`

Nous allons ajouter Bootstrap sur le site, pour ce faire allez sur [le site de Bootstrap](https://getbootstrap.com/), cliquez sur `Download` en bas de l'écran (pas de la page), appuyez ensuite sur le bouton Download, vous allez récupérer un fichier zip. 

Une fois télécharger ouvrez le, cherchez le dossier css et récupérez les fichiers `bootstrap.min.css` et `bootstrap.min.css.map` et mettez-les dans le dossier `css` que vous venez de créer. 

Nous allons faire pareil avec l'autre dossier dans le zip, allez dans le dossier `js` récupérez `bootstrap.bundle.min.js` et `bootstrap.bundle.min.js.map` et mettez les dans le dossier `js` que vous avez créér.

Nous allons maintenant créer un fichier `style.css` dans le dossier `css` et un fichier `scripts.js` dans le dossier `js`

Une fois tout cela fais, il ne reste plus qu'à mentionner ces nouveaux fichiers dans le fichier `base.html.twig` pour ce faire nous allons d'abords

Ajouter le code ci-dessous au-dessus du block `stylesheets`

```twig
<link rel="stylesheet" href="{{ asset('assets/css/bootstrap.min.css') }}" >
```

Et le code ci-dessous en dessous du block `stylesheets`

```twig
<link rel="stylesheet" href="{{ asset('assets/css/style.css') }}" >
```

Cela permet d'instaurer une hiérarchie du CSS.

Et pour le JavaScript c'est pareil

Ajouter le code ci-dessous au-dessus du block `javascripts`

```twig
<script src="{{ asset('assets/js/boostrap.bundle.min.js') }}" defer></script>
```

Et en dessous

```twig
<script src="{{ asset('assets/js/scripts.js') }}" defer></script>
```

---

<h2 align="center">Mise en place des <i>partials</i></h2>

Le fichier `base.html.twig` n'étant pas fais pour contenir énormément de code nous allons séparer certains éléments du code dans différents fichiers, pour ce faire nous allons faire un dossier dans `templates` que nous allons appeler `_partials`

Dans ce dossier nous allons créer deux fichiers `_nav.html.twig`*(ce seras notre barre de navigation)* et `_footer.html.twig`*(ce seras notre bas de page)* 

Maintenant que ces fichiers sont créés, nous avons juste à ajouter deux lignes dans le fichier `base.html.twig`

La première avant le block `body`

```twig
{% include "_partials/_nav.html.twig" %}
```

Et le deuxième après le block `body`

```twig
{% include "_partials/_footer.html.twig" %}
```

---

<h2 align="center">Mise en place de la NavBar</h2>

Pour mettre en place une navbar rien de plus simple, nous allons prendre le tutoriel de bootstrap sur [Comment faire une navbar](https://getbootstrap.com/docs/5.2/components/navbar/) 

```html
<nav class="navbar navbar-expand-lg bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">Navbar</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
        <li class="nav-item">
          <a class="nav-link active" aria-current="page" href="#">Home</a>
        </li>
        <li class="nav-item">
          <a class="nav-link" href="#">Link</a>
        </li>
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false"> Dropdown </a>
          <ul class="dropdown-menu">
            <li>
              <a class="dropdown-item" href="#">Action</a>
            </li>
            <li>
              <a class="dropdown-item" href="#">Another action</a>
            </li>
            <li>
              <hr class="dropdown-divider">
            </li>
            <li>
              <a class="dropdown-item" href="#">Something else here</a>
            </li>
          </ul>
        </li>
        <li class="nav-item">
          <a class="nav-link disabled">Disabled</a>
        </li>
      </ul>
      <form class="d-flex" role="search">
        <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
        <button class="btn btn-outline-success" type="submit">Search</button>
      </form>
    </div>
  </div>
</nav>
```

Et mettez ceci dans le fichier `_nav.html.twig`

---

<h2 align="center">Dernière modification</h2>

Pour éviter des problèmes avec le titre des pages de notre site nous pouvons modifier la balise `<title>` de `base.html.twig`.

Nous allons remplacer cette balise par

```twig
<title>{% block title %}Accueil{% endblock %} - Site e-commerce Symfony 6</title>
```
