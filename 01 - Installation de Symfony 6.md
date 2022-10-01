<h1 align="center">1 - Installation de Symfony 6</h1>

Lien [ICI](https://www.youtube.com/watch?v=kuKb3VfcTWE&list=PLBq3aRiVuwyzI0MT4LhvwqkVenz5pF_DM)

---

Pour commencer il vous faudra [PHP](https://www.php.net/downloads.php), [Composer](https://getcomposer.org/download/), [Symfony CLI](https://symfony.com/download) et [MySQL](https://dev.mysql.com/downloads/cluster/)

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

Votre installation est un succès.

<h3 align="center"><a href="#setup-end">Continuer</a></h3>

---

<h2 align="center"><a id="setup-windows">Windows</a></h2>

---

<h2 align="center"><a id="setup-macos">MacOS</a></h2>

---

<h2 align="center"><a id="setup-end">Après l'installation</a></h2>