<h1 align="center">1 - Installation de Symfony 6</h1>

Lien [ICI](https://www.youtube.com/watch?v=kuKb3VfcTWE&list=PLBq3aRiVuwyzI0MT4LhvwqkVenz5pF_DM)

---

Pour commencer il vous faudra [PHP](https://www.php.net/downloads.php), [Composer](https://getcomposer.org/download/), [Symfony CLI](https://symfony.com/download), [MySQL](https://dev.mysql.com/downloads/cluster/) et Git

Choisissez dépendamment de votre OS

| [Linux](#setup-linux) | [Windows](#setup-windows) | [MacOS](#setup-macos) |
|:---------------------:|:-------------------------:|:---------------------:|

---

<h2 align="center"><a id="setup-linux">Linux</a></h2>

Avant toute chose veillez à bien mettre à jour votre machine

<h4 style="color:red;" align="center">
Vous devez être en root pour les prochaines commandes
</h4>

```shell
apt update
```

<h3 align="center">PHP</h3>

Pour Symfony 6 il vous faudra PHP 8, pour cela il va falloir ajouter le dépôt où ce trouve PHP. 

```shell
#Installation de dépendances
apt install -y lsb-release ca-certificates apt-transport-https software-properties-common gnupg2
#Ajout du dépôt dans la liste des sources
add-apt-repository ppa:ondrej/php
#Met à jour votre machine
apt update -y
#Installation de PHP et de ses dépendances
apt install php8.1 -y
apt install php8.1-{mysql,cli,common,imap,ldap,xml,fpm,curl,mbstring,zip} unzip -y
```

<h3 align="center">Composer</h3>

Pour installer Composer sur Linux rien de plus simple, rendez-vous sur [le site de composer](https://getcomposer.org/download/) et utilisez les commandes à votre disposition.

![](https://raw.githubusercontent.com/H4zeSt0rm/ECommerce/master/assets/01%20-%20Installation%20de%20Symfony%206/composer_php_cli.png)

1. Télécharge Composer

2. Déplace Composer dans `/usr/local/bin` pour avoir un accès global à l'outils (si vous êtes en **root** enlevez le `sudo` dans la commande)

<h3 align="center">Symfony CLI</h3>

Pour installer Symfony CLI rien de plus simple il suffit de suivre les instruction que vous donne Symfony eux même dépendamment de votre distribution Linux [ICI](https://symfony.com/download) (comme pour au dessus, si jamais vous êtes déjà en root, retirez `sudo` des commandes)

<h3 align="center">MySQL (<i>MariaDB</i>)</h3>

Car l'installation de MySQL peut être complexe et prise de tête nous allons utiliser MariaDB pour ce projet.

```shell
#Met à jour la machine si ce n'est pas déjà fait
apt update
#Installe MariaDB Server
apt install mariadb-server
```

Pour tester l'installation faites `mysql` dans votre terminal, si avez une réponse du type

```shell
Maria DB [(none)]>
```

votre installation est un succès.

<h3 align="center">Git</h3>

Pour installer Git rien de plus simple

```shell
apt install git-all
```

<h3 align="center"><a href="#setup-end">Continuer</a></h3>

---

<h2 align="center"><a id="setup-windows">Windows</a></h2>

Pour installer PHP, Composer et Symfony sur Windows, ce n'est pas compliqué nous allons récupérer les "binaries". Pour MySQL c'est une autre histoire.

Pour commencer allez dans `Documents` et faites un dossier `CLIs` ça seras important pour plus tard.

Vous allez avoir besoin d'une fenêtre spéciale pour de futures manipulation, pour l'ouvrir faites: `Win+R`, écrivez `SystemPropertiesAdvanced` et faites entrer. Cliquez sur `Variables d'environnement...`, dans la partie du dessus cherchez `Path` en nom de variable. Faites `Modifier` et gardez cette fenêtre ouverte nous en aurons besoin plus tard.

<h3 align="center">PHP</h3>

Pour PHP rendez vous sur [le site de PHP](https://www.php.net/downloads.php) pour récupérer les "binaries". Cliquez sur 

![](./assets/01%20-%20Installation%20de%20Symfony%206/php_win_downloads.png)

Téléchargez le fichier zip

![](./assets/01%20-%20Installation%20de%20Symfony%206/php_chose_zip.png)

Quand le zip est téléchargé extrayez le dans un dossier `php<votre_version>`

Allez sur la fenêtre `Variables d'environnements...` et faites `Nouveau` et écrivez la ligne ci-dessous:

```shell
%userprofile%\Documents\CLIs\php<votre_version>\
```

<h3 align="center">Composer</h3>

Pour composer vous avez deux choix:

- L'installer avec `Composer-Setup.exe`

- Installer les `binaries`

Dans les deux cas vous devez aller sur [le site de composer](https://getcomposer.org/download/)

Si vous voulez `Composer-Setup.exe` continuez ici, sinon allez [ici](#use_composer_binaries).

Vous avez un lien vers `Composer-Setup.exe`

![](./assets/01%20-%20Installation%20de%20Symfony%206/composer_setup_exe.png)

Pendant l'installation il vous seras demandé la position de PHP, renseignez le dossier fait précédemment.

Si vous avez l'erreur suivante il faudra installer des bibliothèques Microsoft supplémentaires.

![erreur_composer_setup_lib.jpg](./assets/01%20-%20Installation%20de%20Symfony%206/erreur_composer_setup_lib.jpg)

Vous pouvez installer la bibliothèque nécessaire [ici](https://visualstudio.microsoft.com/fr/downloads/)

![erreur_composer_setup_where_lib.jpg](./assets/01%20-%20Installation%20de%20Symfony%206/erreur_composer_setup_where_lib.jpg)

Quand le téléchargement est fini, vous n'avez plus qu'à installer.

![microsoft_visual_c++_installeur.jpg](./assets/01%20-%20Installation%20de%20Symfony%206/microsoft_visual_c++_installeur.jpg)

<a id="use_composer_binaries"></a>

Si vous voulez les `binaries` vous pouvez descendre plus bas sur la page jusqu'à `Manual Download`. Prenez `Latest Stable`

![](./assets/01%20-%20Installation%20de%20Symfony%206/composer_binaries.png)

Vous aurez un `composer.phar`. Faites un dossier `composer` dans le dossier`CLIs` précédemment créé et mettez `composer.phar` à l'intérieur.

Vous allez devoir faire un autre fichier dans ce dossier. `composer.bat`, vous allez écrire  le texte ci-dessous à l'intérieur

```batch
@php "%~dp0composer.phar" %*
```

Faites ensuite comme pour PHP. Ouvrez une **nouvelle** `Invite de commande` et faites la commande suivante.

Allez sur la fenêtre `Variables d'environnements...` et faites `Nouveau` et écrivez la ligne ci-dessous:

```shell
%userprofile%\Documents\CLIs\composer\
```

<h3 align="center">Symfony CLI</h3>

Pour Symfony CLI vous devez allez sur [le site de Symfony](https://symfony.com/download) et choisir Windows.

N'installez pas Scoop, c'est une perte de temps. Prenez plutôt les `binaries`. Choisissez `amd64`, une fois le zip téléchargé, comme pour PHP et Composer, faites un dossier dans `CLIs` nommez le `symfony_cli` et mettez le `symfony.exe` présent dans le zip dans le dossier.

Ouvrez ensuite une **nouvelle** `Invite de commande`. Et exécutez cette commande.

Allez sur la fenêtre `Variables d'environnements...` et faites `Nouveau` et écrivez la ligne ci-dessous:

```shell
%userprofile%\Documents\CLIs\symfony_cli\
```

<h3 align="center">MySQL</h3>

Pour MySQL des gens proposent d'utiliser Xamp, Wamp ou Mamp mais ces dernières peuvent poser des problèmes, nous allons donc installer directement MySQL.

Les explications étant tellement grandes, je vous redirige sur mon tuto pour installer [MySQL sur Windows](https://github.com/H4zeSt0rm/CoursPHP/blob/master/Installer%20MySQL%20sur%20Windows.md).

Une fois l'installation terminée, si vous avez besoin de la commande `mysql` dans vos `Invites de commandes` et que vous n'avez pas touché au dossier d'installation de MySQL.

Allez dans le dossier suivant:

```
C:\Program Files\MySQL\
```

Récupérez le nom du dossier commençant par `MySQL Server`.

Allez sur la fenêtre `Variables d'environnements...` et faites `Nouveau` et écrivez la ligne ci-dessous:

```
C:\Program Files\MySQL\MySQL Server ?\bin
```

<h3 align="center">Git</h3>

Pour installer Git il faut juste aller sur [le site de Git](https://git-scm.com/download/win)

![](./assets/01%20-%20Installation%20de%20Symfony%206/git_homepage.png)

Choisissez dépendamment de votre ordinateur. `Standalone` étant une version installé sur votre ordinateur et `Portable` comme son nom l'indique est une version installable sur une clé USB ou un disque dur.

Vous pouvez laisser les paramètres par défaut pour l'installation.

Une fois l'installation faite première chose à faire sont les commandes ci-dessous

```shell
git config --global user.name "Votre Pseudonyme Github"
git config --global user.email "Votre Email Github"
```

<h3 align="center"><a href="#setup-end">Continuer</a></h3>

---

<h2 align="center"><a id="setup-macos">MacOS</a></h2>

---

<h2 align="center"><a id="setup-end">Après l'installation</a></h2>

Vous pouvez maintenant initialiser un projet Symfony, je vous conseil de faire un dossier dans `Documents` que vous pouvez appeler `Projets Symfony`.

Ouvrez ensuite une `Invite de commande` dans le dossier et faites la commande suivante.

```shell
symfony new ecommerce_sf6 --version="6.0.*" --webapp
```

Nous utiliserons la version 6.0.* de Symfony pour ne avoir la même version que dans le tuto vidéo.

<h1 align="center">Fin</h1>