# Installation et configuration d'un serveur web

## Étape 1 - Pré-requis

```
# apt-get install sudo
```
Après vous être connecté à votre système Debian, mettez à niveau les paquets actuels vers la dernière version disponible.
```
# apt-get update
# apt-get upgrade
```
Installez également les packages ci-dessous sur votre système requis pour les commandes suivantes de ce didacticiel.
```
# apt install ca-certificates apt-transport-https
```

## Étape 2 - Installation d'Apache2
Les référentiels de base de Debian 9 contiennent des paquets Apache 2.4. Vous pouvez simplement installer les paquets Apache2 en exécutant les commandes suivantes sur votre système Debian 9.
```
# apt-get install apache2
```

## Étape 3 - Installation de MySQL
Le référentiel de base de Debian 9 contient MariaDB (remplacement de MySQL) comme serveur de base de données par défaut. Si vous êtes d'accord pour utiliser MariaDB, exécutez les commandes pour installer sinon suivez le [tutoriel d'installation du serveur MySQL](https://tecadmin.net/install-mysql-server-on-debian9-stretch/).
```
# apt-get install mysql-client mysql-server
```
 Avec MySQL depuis Bionic 18.04, et MariaDB depuis Xenial 16.04, l'authentification de l'utilisateur root de MySQL se fait au moyen du plugin auth_socket, donc avec sudo.
 
Si vous avez besoin d'un accès global à vos bases de données depuis un même compte, la solution conseillée est donc de créer un nouvel utilisateur et de lui attribuer tous les privilèges : 
```
# mysql
```
Puis dans la console MySQL : 
```
> GRANT ALL ON *.* TO 'nom_utilisateur_choisi'@'localhost' IDENTIFIED BY 'mot_de_passe_solide' WITH GRANT OPTION;
> FLUSH PRIVILEGES;
> QUIT;
```
En remplaçant évidemment nom_utilisateur_choisi et mot_de_passe_solide dans cette requête. 

Connectez-vous ensuite via phpmyadmin à vos base de données et supprimez les autres utilisateurs.

![illustration phpmyadmin](https://i.ibb.co/cTj00dk/phpmyadmin.png)

## Étape 4 - Installation de PHP
Les référentiels système par défaut de Debian 9 contiennent une ancienne version PHP. Pour installer la dernière version de PHP, ajoutez un PPA tiers à votre système. Exécutez la commande ci-dessous pour ajouter PPA à votre système.
```
# wget -q https://packages.sury.org/php/apt.gpg -O- | sudo apt-key add -
# echo "deb https://packages.sury.org/php/ stretch main" | tee /etc/apt/sources.list.d/php.list
```
Installez ensuite la dernière version de PHP sur Debian 9.
```
# apt update
# apt install php php-mysql libapache2-mod-php
```

## Étape 5 - Vérification de la configuration
Pour vérifier la configuration de LAMP sur Debian 9, créez un script PHP avec la fonction phpinfo () sous la racine du document Apache. Pour ce faire, modifiez le fichier **/var/www/html/info.php** dans votre éditeur de texte préféré et ajoutez le contenu ci-dessous au fichier et enregistrez-le.
Contenu:
```php
<?php
  phpinfo();
?>
```
Accédez maintenant au fichier info.php dans le navigateur Web en utilisant l'adresse IP de votre système.

![illustration info.php](https://i.ibb.co/NtjBGFF/info-php.jpg)

## Étape 6 - Installation de Phpmyadmin

```
# apt-get install phpmyadmin
```
Cochez **Non** pour utiliser dbconfig-common et sélectionnez l'option [x] **apache2**.

## Étape 7 - Créer un nouvel utilisateur

```
# adduser server
$ cd /home/server
```

## Étape 8 - Installation du serveur FiveM

```
# apt-get install git
# apt-get install xz-utils
```
Pour FiveM, en version linux, rendez-vous sur <https://runtime.fivem.net/artifacts/fivem/build_proot_linux/master/>. Copiez ensuite le lien de la dernière mise à jour.

1 ) Remplacez le lien ci-dessus par celui copié auparavant
```
# wget https://runtime.fivem.net/artifacts/fivem/build_proot_linux/master/.../fx.tar.xz
```
2 ) Pour extraire le fichier
```
# tar xvfJ fx.tar.xz
```
3 ) Récupérer la base de FiveM
```
# git clone https://github.com/citizenfx/cfx-server-data.git server-data
```
4 ) Supprimez l'archive
```
# rm fx.tar.xz
```
5 ) Rendra des fichiers éditable par l'utilisateur 'server'
```
# chown -R server:server /home/server/
```

## Étape 9 - Exécutez le serveur
```
$ cd /home/server/server-data
```
```
$ bash /home/server/run.sh +exec server.cfg
```

## Pour faire plusieurs écrans
#### 1) Installez le package
```
# apt-get install screen
```
#### 2) Utilisation de celui-ci
Pour créer un écran virtuel :
```
# screen -R server
```
Pour listez les différents écrans :
```
# screen -ls
```

Copyright 2020 [Darkylex](https://darkylex.com/ "Darkylex - Site officiel")

*Source: <https://tecadmin.net/install-lamp-stack-debian-9-stretch/>, <https://doc.ubuntu-fr.org/phpmyadmin>, et moi même*
