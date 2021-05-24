---
layout: page
title: htaccess et webfont
permalink: htaccess-webfont.html
---

Deux cas de figures pouvant se présenter avec des Webfonts

## 1) Autoriser le chargement de webfont depuis un autre domaine

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

Ce cas de figure se présente notamment pour l'utilisation de **webfonts** commerciales. Certaines fonderies peuvent vous demander de protéger les fontes contre le piratage, par une restriction .htaccess.

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

