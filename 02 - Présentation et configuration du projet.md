<h1 align="center">2 - Présentation et configuration du projet</h1>

Lien [ICI](https://www.youtube.com/watch?v=kpSYFMV4eJc&list=PLBq3aRiVuwyzI0MT4LhvwqkVenz5pF_DM)

---

[Voici à quoi va ressembler la base de donnée](https://dbdiagram.io/d/61643981940c4c4eec8f40a5)

<img src=".\assets\02%20-%20Présentation%20et%20configuration%20du%20projet\database_diagram.png" title="" alt="" width="722">

Première chose à faire après avoir créer un projet, c'est de ce déplacer à l'intérieur

```shell
cd ecommerce_sf6
```

<h2 align="center">Dépendances</h2>

Comme ça toutes vos commandes seront exécutés dans ce répertoire. Vous pouvez maintenant faire cette commande pour vérifier si il y a des dépendances optionnels qui ne sont pas respectés.

```shell
symfony check:requirements
```

Si vous avez des **W** sur la ligne de **.** c'est qu'il y a des extensions et des dépendances qui ne sont pas activés ou configurés correctement.

Suivez les instructions dans le cmd pour régler les problèmes séparément.

---

<h2 align="center">Lien avec MySQL</h2>

Avant tout, il faut cloner le `.env` dans votre dossier, renommez la copie `.env.local`, cela vous permettra de faire de modifications de configuration qui ne seront pas synchro avec GitHub.

Pour lier le site à MySQL si vous avez suivis le dernier tuto, vous devez chercher la ligne  

`DATABASE_URL="mysql:`, retirez le **#** au début  et ajoutez un **#** au début de la ligne `# DATABASE_URL="postgresql:` l'URL pour MySQL est composé comme ceci

![](.\assets\02%20-%20Présentation%20et%20configuration%20du%20projet/explication_databaseurl.png)

- Rouge: Le type de base de donnée

- Bleu: Login de votre utilisateur MySQL

- Vert: Mot de passe de votre utilisateur MySQL

- Jaune: Adresse de votre base de donnée

- Cyan: Nom de la base de donnée

Vous allez devoir changer les valeurs selon vos besoins

Blue et Vert sont les valeurs que vous avez définis à l'initialisation de MySQL

Cyan je vous conseil de mettre `ecommerce_sf6` comme nom de base de donnée

---
