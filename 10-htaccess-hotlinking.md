---
layout: page
title: htaccess et hotlinking
permalink: hotlinking.html
---

Ce cas d'utilisation du HTACCESS vise la prévention du "hotlinking", càd l'accès direct aux images d'un site (p.ex. pour les insérer dans un forum de discussion).

Voici un exemple de code HTACCESS qui retourne une erreur pour tout accès direct à un fichier image:

```
RewriteEngine on
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www\.)example.com/.*$ [NC]
RewriteRule \.(gif|jpg|jpeg|png|pdf)$ - [F]
```

La 2ème ligne signifie qu'on autorise les "blank referrers", les accès qui ne fournissent aucune information de provenance. Ceci est le cas d'usagers se trouvant derrière un firewall.

La 3ème ligne indique le domaine autorisé à accéder aux images. Il faut remplacer example.com par votre domaine.

La 4ème ligne empêche l'accès à une série de types de fichier.

Voici un exemple similaire, qui retourne une image prédéfinie (probablement avec un message explicatif ou ironique):

```
RewriteEngine on
RewriteCond %{HTTP_REFERER} !^$
RewriteCond %{HTTP_REFERER} !^http://(www\.)example.com/.*$ [NC]
RewriteRule \.(gif|jpg)$ http://www.example.com/no-hotlinking.gif [R,L]
```

Exemple d'image pouvant être utilisée:

![](img/no-hotlinking.png)