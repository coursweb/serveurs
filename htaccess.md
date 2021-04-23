---
layout: page
title: htaccess
permalink: /htaccess.html
---

Explications concernant le fichier `.htaccess`.

## A quoi il sert 

.htaccess est un fichier qui permet de créer des régles dans un site, par exemple afficher la liste des fichiers sur un serveur à la place d'un message d'erreur.

On peut également l'utiliser pour rediriger une adresse vers un autre site.

Ou bloquer certaines adresses IP d'accéder au site.

## Où le placer ?

Il se place généralement à la racine du dossier du site.

Ce fichier fonctionnera uniqument sur un site tournant sur un serveur Apache. Il sera sans effet sur un hébergement Github Pages, ou Netlify.

## Comment l'utiliser ?

Il suffit de créer un document texte. Ensuite, enregistrer le fichier en avec le nom ".htaccess" et le placer à la racine du dossier du site.

## Opérations fréquentes

### Afficher la liste des fichiers

Pour afficher la liste des dossiers et fichier, ajoutez la ligne:

`Options +Indexes`.

### Autres opérations

- Optimisation de vitesse (compression gzip)
- Durée du cache: mise en cache avec une durée spécifiée par format.
- [Forcer des redirections](redirection-htaccess.html).
- Empêcher l'accès à certains contenus.

## Références

Une bonne référence: le *HTML5 Boilerplate* donne de nombreux conseils d'optimisation de vitesse.

[https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess](https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess)



## Les "rewrite rule flags"

Explication de quelques paramètres utilisés dans les règles d'écriture:

- **[F] : forbidden** - la ressource est inaccessible, le serveur donnera un message "403 Forbidden".
- **[L] : last** - Lorsque le drapeau [L] est présent, mod_rewrite arrête le traitement du jeu de règles.
- **[NC] : nocase** - traitement insensible à la casse. Par exemple, .jpg aussi bien que .JPG seront acceptés. 

Pour plus d'informations, voir :

* [Documentation sur Apache.org](https://httpd.apache.org/docs/current/rewrite/flags.html)