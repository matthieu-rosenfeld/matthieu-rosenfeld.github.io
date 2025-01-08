---
title: Projet - Front de l'API (gestion d'événements)
lang: fr
---

## Dernier rendu des projets Framework Web

Le projet se fera avec en **trinôme** et s'intéressera au développement du **front** de l'**API REST** de **gestions d'événements**. Pour la réalisation du projet, il faudra se baser sur le contenu des TDs Vue.js. 
Vous pouvez modifier les deux précédents projets si besoin. 
Les modifications faites au projet annuaire, ne seront pas évaluées, mais sont autorisées si besoin. 
Par contre, **la version évaluée de l'API REST sera la version que vous rendrez à ce rendu**. Les consignes pour [l'API REST sont toujours disponibles à cette adresse](https://mgasquet.github.io/R5.A.05-ProgrammationAvancee-Web/tutorials/projet2). Les consignes de réalisation du projet front en vue.js sont dans la suite de ce document.

Pour ce dernier rendu, il faudra **déployer vos trois projets sur le serveur webinfo**. Des consignes pour vous aider sont à la fin de ce document.

La deadline de ce dernier rendu est le **vendredi 20 décembre 2024 à 21h00**.
Le projet sera à rendre sur **Moodle** [à cette adresse](https://moodle.umontpellier.fr/course/view.php?id=31511).
Un seul membre du trinôme dépose une archive **zip** nommée selon le format : `NomPrenomMembre1-NomPrenomMembre2-NomPrenomMembre3.zip`. 

Cette archive devra contenir :

* L'ensemble des sources des trois projets organisés comme dans le docker du cours pour nous faciliter le déploiement en local (c'est-à-dire, les deux projets Symfony dans le dossier `public_html` et le projet Vue.js dans le dossier `workspace`). Attention à ne pas inclure les répertoires **vendor**, **var** des projets Symfony et les dossiers **node_modules** et **dist** du projet vue.

* Une manière de faire fonctionner une BD non vide quand le code tourne en local. Plusieurs solutions sont possibles :
    * utiliser SQLite et nous fournir le fichier de la BD,
    * nous fournir un fichier d'import MySQL,
    * fournir une commande PHP qui peuple la base,
    * connecter la version dev à la BD déployé sur webinfo.

* Un fichier **README** qui contient :

    * Les URL où les trois projets sont déployés.

    * Le lien du dépôt git où le code source de l'application est hébergé.

    * Un récapitulatif de l’investissement de chaque membre du groupe dans les deux derniers projets (globalement, qui a fait quoi).

    * Éventuellement, des indications supplémentaires s'il y a des choses particulières à faire pour lancer et tester vos applications en local (autrement que de lancer le serveur, faire les `composer/npm install` des projets, configurer et générer la base de données, etc.).
    
    * Une explication sur comment peupler la BD. 

    * Tout autre commentaire que vous jugez pertinent.


## Front en vue.js pour l'API REST
L'objectif est de développer un front pour les utilisateurs basiques à l'aide de `Vue.js`. On imagine que les administrateurs et les organisateurs d'événement ont accès à une autre interface. Ce front devra donc permettre d'utiliser les fonctionnalités suivantes :

* Permettre à des utilisateurs de s'**inscrire**, de s'**authentifier**, de **modifier** les informations de leurs comptes, de supprimer leur compte, etc.

* N'importe quel utilisateur (connecté ou non) doit pouvoir consulter la liste des événements organisés

* Un utilisateur connecté doit pouvoir consulter la liste des événement auquel il est inscrit, et s'inscrire ou se désinscrire d'un événement. 

* Ce projet sera aussi relié au projet annuaire : un utilisateur aura la possibilité d'ajouter son code identifiant du projet annuaire pour que certaines informations de l'API de l'annuaire soient utilisées sur la page profil (en bonus on pourrait aussi l'utiliser pour remplir automatiquement certains champs lors de l'inscription).

* Vous pouvez si vous le désirez ajouter d'autres fonctionnalités, notamment pour les autres rôles, mais cela ne vous est pas demandé. 

Vous serez attentif à ce que votre projet contienne :
* des routes nommées,
* une gestion correcte de la connexion / déconnexion / rafraichissement du token s'il expire pendant la navigation,
* un découpage pertinent en vues et composants,
* une gestion des différentes erreurs possibles lors des requêtes à l'API,
* un code correctement typé avec TypeScript,
* des notifications (messages flashes) pour améliorer la navigation.

Vous pouvez utiliser un framework CSS.

Pour vous guider, voici les catégories utilisées dans notre barème de l'an passé pour la notation du projet Vue :
* Le déploiement fonctionne
* Linter ok + Typage valide
* Utilisation correcte des routes
* Utilisation correcte des composants
* Liens avec l'API et stockage des données
* Gestion de la connexion
* Utilisation des messages flashes
* Utilisation de l'annuaire
* Impression générale
* Bonus malus


## Hébergement des applications
 Vous devrez les héberger dans le dossier `public_html` d'un des membres de l'équipe (pas nécessairement le même pour toutes les applications).

 Attention, vous n'avez qu'une seule base MySQL chacun et vous ne pourrez pas déployer vos deux projets Symfony sur la même base. Vous pouvez soit utiliser la base d'une autre membre du groupe pour le deuxième projet ou simplement utiliser des bases SQLite.


Pour pouvoir accéder à votre *home* à distance et y déposer des fichiers, il faudra d'abord trouver vos identifiants de connexion login et mot de passe.

Vous pourrez utiliser FileZilla (il faudra certainement l'installer) pour déposer des fichiers dans votre dossier `~/public_html` sur le serveur de l'IUT. Pour vous connecter, il faudra choisir comme protocole `SFTP`, comme hôte `ftpinfo.iutmontp.univ-montp2.fr` et vous devrez utiliser votre login et mot de passe. Vous pouvez alors facilement déplacer des fichiers de votre machine vers votre `home`.


Vous aurez surtout besoin de vous connecter en `ssh` pour pouvoir exécuter les commandes nécessaires au bon déploiement. Vous pouvez d'ailleurs utiliser git depuis `ssh` pour récupérer vos projets sans utiliser FTP. Pour les projets Symfony, il faudra notamment faire les `composer install` et probablement créer les bases. Pour le projet Vue, vous pouvez normalement copier directement la version build du projet.

Sous Linux, la commande vous permettant de vous connecter au serveur est la suivante (en remplaçant `mon_login_IUT` par votre login)

```sh
ssh mon_login_IUT@162.38.222.93 -p 6666
```

Il faudra ensuite entrer votre mot de passe et répondre "yes" à la question "Are you sure you want to continue connecting". Vous êtes alors connectés en `ssh` et les commandes que vous tapez sont exécutées sur la machine cible. Vous pouvez alors procéder au déploiement de votre site.


Il faudra aussi certainement donner les droits au serveur Apache de lire vos fichiers. Pour cela depuis le terminal connecté en `ssh` vous pourrez utiliser les deux commandes suivantes :

```sh
   # On modifie (-m) récursivement (-R) les droits r-x
   # de l'utilisateur (u:) www-data
   setfacl -R -m u:www-data:r-x ~/public_html
   # On fait de même avec des droits par défaut (d:)
   # (les nouveaux fichiers prendront ces droits)
   setfacl -R -m d:u:www-data:r-x ~/public_html
```

Il faudra bien vérifier que vos applications sont accessibles depuis l'extérieur de l'IUT sur l'adresse : [http://webinfo.iutmontp.univ-montp2.fr/~login_depot/sous-adresse-du-projet](http://webinfo.iutmontp.univ-montp2.fr/~login_depot/sous-adresse-du-projet).


Des sources complémentaires sur comment se connecter en FTP et en SSH à `public_html` (pour y déposer des fichiers) :

* [https://iutdepinfo.iutmontp.univ-montp2.fr/intranet/votre-espace-de-travail/](https://iutdepinfo.iutmontp.univ-montp2.fr/intranet/votre-espace-de-travail/)
* [https://iutdepinfo.iutmontp.univ-montp2.fr/intranet/acces-aux-serveurs/](https://iutdepinfo.iutmontp.univ-montp2.fr/intranet/acces-aux-serveurs/)
* [https://iutdepinfo.iutmontp.univ-montp2.fr/intranet/partager-public_html/](https://iutdepinfo.iutmontp.univ-montp2.fr/intranet/partager-public_html/)
* [https://docs.google.com/document/d/1rLb4QWt0uOxE8IuLqeUxjZTivMIA6KbLzieNvsSa_p0/edit#heading=h.l1xavd57lfgb](https://docs.google.com/document/d/1rLb4QWt0uOxE8IuLqeUxjZTivMIA6KbLzieNvsSa_p0/edit#heading=h.l1xavd57lfgb)


## Déroulement du projet et accompagnement

Comme pour les autres projets, il faudra aussi utiliser et bien organiser un dépôt git. N'oubliez pas que vous pouvez utiliser [le Gitlab du département](https://gitlabinfo.iutmontp.univ-montp2.fr).

N'hésitez pas à poser des questions à votre enseignant chargé de TD et à montrer votre avancement ! Bons projets.