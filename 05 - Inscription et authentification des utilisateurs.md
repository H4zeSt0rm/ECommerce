<h1 align="center">5 - Inscription et authentification des utilisateurs</h1>

Lien [ICI](https://www.youtube.com/watch?v=INfHFDIjgrw&list=PLBq3aRiVuwyzI0MT4LhvwqkVenz5pF_DM)

---

<h2 align="center">Création d'un authentificateur</h2>

Pour faire un authentificateur il faut faire cette commande

```shell
symfony console make:auth
```

- 1 *nous voulons un formulaire pré-fait*

- UsersAuthenticator

- 

- 

- 

Rendez vous dans `/src/Controller/SecurityController.php` et remplacez 

`@Route("/login..` par `@Route("/connexion..`

Si vous essayez d'aller sur la page `/connexion` de votre site vous verrez un formulaire de connexion qui utilise Bootstrap, à vous de modifier la page à votre goût

---

<h2 align="center">Création d'un formulaire d'inscription</h2>

Pour faire un formulaire d'inscription c'est cette commande

```shell
symfony console make:registration-form
```

- 

- no

- 

- 

Comme pour `SecurityController` allez dans `/src/Controller/RegistrationController.php` et remplacez

`#[Route('/register..` par `#[Route('/inscription..`

La page `/register` a été crée.

Mais si nous essayons de nous connecter nous aurons une erreur car la majorité des information qu'un `Users` a besoin ne sont pas préciser dans ce formulaire.

Pour ce faire rendez vous dans le fichier `/src/Form/RegistrationFormType.php`
A l'intérieur vous allez voir qu'il y a 3 champs 

- email

- agreeTerms

- password

Comme pour `email` nous allons ajouter plusieurs champs avec `add()`

- lastname

- firstname

- adress

- zipcode

- city

Nous allons aussi remplacer `agreeTerms` en `RGPDConsent`

Mais ce n'est pas fini, le formulaire est en 2 parties, rendez vous dans `/templates/registration/register.html.twig`

Vous allez retrouver les mêmes éléments que dans l'autre fichier.

Nous allons donc devoir ajouter les nouveaux éléments et en modifier 1.
En effet il faut modifier `agreeTerms` en `RGPDConsent` ici aussi si nous ne voulons pas d'erreur.

Nous allons faire quelques modifications à cette page pour la mettre en page *nous allons juste changer ce qui se trouve dans le block body*

```twig
<section class="container">
    <div class="row">
        <div class="col">
            <h1>Inscription</h1>
            {{ form_start(registrationForm) }}
                <fieldset class="mb-3">
                    <legend>Mon identité</legend>
                    {{ form_row(registrationForm.lastname) }}
                    {{ form_row(registrationForm.firstname) }}
                    {{ form_row(registrationForm.email) }}
                </fieldset>    
                <fieldset class="mb-3">
                    <legend>Mes coordonnées</legend>
                    {{ form_row(registrationForm.address) }}
                    {{ form_row(registrationForm.zipcode) }}
                    {{ form_row(registrationForm.city) }}
                </fieldset>
                {{ form_row(registrationForm.plainPassword) }}
                {{ form_row(registrationForm.RGPDConsent) }}
                <button type="submit" class="btn btn-primary btn-lg my-3">M'inscrire</button>
            {{ form_end(registrationForm) }}
        </div>
    </div>
</section>
```

Retournons maintenant dans `/src/Form/RegistrationFormType.php` nous allons modifier des attributs, nous permettant d'afficher des labels différents et d'ajouter du style au inputs du formulaire.

Pour `->add('email')` nous allons ajouter un `EmailType::class` 
`->add('email',EmailType::class)`

Pour tous les autres

- lastname

- firstname

- adress

- zipcode

- city

Nous allons leurs ajouter `TextType::class` nous allons ensuite ajouter à tout le monde des attributs communs. Donc pour chaque `add()` *avant RGPDConsent* nous allons ajouter 

```php
, [
    'attr' => [
        'class' => 'form-control',
        'label' => '!DEPEND DE L OBJET!'
    ]
]
```

Il va falloir ajouter ces éléments à RGPDConsent et à plainPassword *notez que RGPDConsent a déjà des attributs donc ajoutez juste à la liste déjà existante*

| RGPDConsent (label) | En m\'inscrivant à ce site j\'accepte... |
| ------------------- | ---------------------------------------- |

Il va falloir ajouter une ligne dans `/src/Entity/Users.php` .

Dans la fonction `__construct()` ajoutez cette ligne

```php
$this->created_at = new \DataTimeImmutable();
```

Cela permettra d'éviter une date de création null à l'inscription.

Allez ensuite dans `/templates/security/login.html.twig` et modifiez

```twig
{% if app.user %}
    <div>
        {# Modifiez en dessous #}     
        Vous êtes connecté(e) en temps que {{ app.user.userIdentifier }}, <a href="{{ path('app_logout') }}">Me déconnecter</a> 
    </div>
{% endif %}
```

---

<h2 align="center">Adaptation de la navbar</h2>

Nous allons ensuite revenir sur la NavBar du dernier tutoriel, nous allons supprimer

```html
<form class="d-flex" role="search">
    <input class="form-control me-2" type="search" placeholder="Search" aria-label="Search">
    <button class="btn btn-outline-success" type="submit">Search</button>
</form>
```

Nous allons le remplacer par ceci

```twig
<ul class="navbar-nav ms-auto mb-2 nb-lg-0">
    {% if app.user %} {# Si tu est connecté #}
        <li class="nav-item">
            <a class="nav-link" href="#">Mon compte</a>
        </li>
        <li class="nav-item">
            <a class="nav-link" href="{{ path('app_logout') }}" > Me déconnecter</a>
        </li>
    {% else %}
        <li class="nav-item">
            <a class="nav-link" href="{{ path('app_register') }}">M'inscrire</a>
        </li>
        <li class="nav-item">
            <a class="nav-link" href="{{ path('app_login') }}">Mon connecter</a>
        </li>
    {% endif %}
</ul>     
```

Et voilà, nous en avons finis avec les formulaires de connexion et d'inscription, tout est en place pour la suite du tutoriel !


