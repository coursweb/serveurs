---
layout: page
title: htaccess
permalink: /htaccess/
---

Explication concernant le fichier `.htaccess`.

Ce fichier, placé dans le dossier racine d'un site tournant sur un serveur Apache, permet d'effectuer un certain nombre de réglages: 

- Optimisation de vitesse (compression gzip)
- Durée du cache: mise en cache avec une durée spécifiée par format.
- Forcer des redirections.
- Empêcher l'accès à certains contenus.

Une bonne référence: le *HTML5 Boilerplate* donne de nombreux conseils d'optimisation de vitesse.

[https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess](https://github.com/h5bp/html5-boilerplate/blob/master/dist/.htaccess)

## Autoriser le chargement de webfont depuis un autre domaine

Certains navigateurs vont refuser de charger une ressource qui est hébergée sur un domaine différent du HTML actuel.

Pourtant cela peut être nécessaire, par exemple dans le cas d'un blog Tumblr pour lequel vous aimeriez utiliser une webfont depuis votre propre domaine.

Voilà le genre de message d'erreur pouvant apparaître dans la console de votre navigateur:

```
Access to font at 'https://example.com/aileron/italic.woff' from origin 'https://example.org' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

Pour autoriser cela, vous devez ajouter un réglage HTACCESS au domaine qui héberge la fonte:

```
<IfModule mod_headers.c>
   Header set Access-Control-Allow-Origin "*"
</IfModule>
```

## Empêcher l'accès à des contenus

Ce cas de figure se présente notamment pour l'utilisation de **webfonts** commerciales. Certaines fonderies demandent de protéger les fontes par une restriction .htaccess.

L'idée est de permettre le chargement uniquement depuis un nom de domaine spécifique: si la fonte est chargée via une feuille de style CSS depuis le domaine example.com, elle sera disponible. Toutes les autres demandes d'accès à la fonte seront refusées - il est donc impossible de télécharger les fichiers. 

Voici un exemple de code .htaccess qui produit cet effet:

```
<FilesMatch "\.(eot|woff|svg|ttf)$">
    RewriteEngine On

    # Let proxy servers with empty referer through
    
    RewriteCond %{HTTP_REFERER}             ^$
    RewriteCond %{HTTP:VIA}                 !^$ [OR]
    RewriteCond %{HTTP:USERAGENT_VIA}       !^$ [OR]
    RewriteCond %{HTTP:FORWARDED}           !^$ [OR]
    RewriteCond %{HTTP:FORWARDED-FOR}       !^$ [OR]
    RewriteCond %{HTTP:X-FORWARDED}         !^$ [OR]
    RewriteCond %{HTTP:X-FORWARDED-FOR}     !^$ [OR]
    RewriteCond %{HTTP:X_FORWARDED_FOR}     !^$ [OR]
    RewriteCond %{HTTP:PROXY_CONNECTION}    !^$ [OR]
    RewriteCond %{HTTP:XPROXY_CONNECTION}   !^$ [OR]
    RewriteCond %{HTTP:HTTP_PC_REMOTE_ADDR} !^$ [OR]
    RewriteCond %{HTTP:HTTP_CLIENT_IP}      !^$
    RewriteRule .* - [PT,L]

    # For everybody else, the referer must match the domains for which this
    # web-font is licensed.
    
    RewriteCond %{HTTP_REFERER} !:\/\/([^.]+\.|)example.com
    RewriteRule .* - [F]

    # Set Cache-Control header to must-revalidate, as we want clients to check
    # against the above rules on each request.
    Header set Cache-Control "must-revalidate"
    
</FilesMatch>
```

Un autre cas de figure similaire vise la prévention du "hotlinking", càd l'accès direct aux images d'un site (p.ex. pour les insérer dans un forum de discussion).

Voici un exemple qui retourne une erreur pour tout accès direct à un fichier image:

```
RewriteEngine on
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www\.)example.com/.*$ [NC]
RewriteRule \.(gif|jpg|jpeg|bmp|zip|rar|mp3|flv|swf|xml|php|png|css|pdf)$ - [F]
```

La 2ème ligne signifie qu'on autorise les "blank referrers", les accès qui ne fournissent aucune information de provenance. Ceci est le cas d'usagers se trouvant derrière un firewall.

La 3ème ligne indique le domaine autorisé à accéder aux images.

La troisième ligne empêche l'accès à une série de types de fichier.

Et voici un exemple similaire, qui retourne une image prédéfinie (probablement avec un message explicatif ou ironique):

```
RewriteEngine on
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www\.)example.com/.*$ [NC]
RewriteRule \.(gif|jpg)$ http://www.example.com/no-hotlinking.gif [R,L]
```

#### Les "rewrite rule flags"

Explication de quelques paramètres utilisés dans les règles d'écriture:

- **[F] : forbidden** - la ressource est inaccessible, le serveur donnera un message "403 Forbidden".
- **[L] : last** - Lorsque le drapeau [L] est présent, mod_rewrite arrête le traitement du jeu de règles.
- **[NC] : nocase** - traitement insensible à la casse. Par exemple, .jpg aussi bien que .JPG seront acceptés. 

Pour plus d'informations, voir :

* [Documentation sur Apache.org](https://httpd.apache.org/docs/current/rewrite/flags.html)