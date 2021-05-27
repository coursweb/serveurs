---
layout: page
title: robots.txt
permalink: robots.html
---


Comment ne pas indexer un fichier dans les moteurs de recherche ?

```
User-agent: Googlebot
Disallow: /"lien du fichier"/
```

Où placer le fichier robots.txt ?

Le fichier doit être placé à la racine du site.

Par exemple: https://exemple.ch/robots.txt

## Exemple: bloquer l'indexation par archive.org

Certains propriétaires de site souhaitent éviter que leur site soit archivé par Internet Archive et accessible via archive.org

Voici le code qui indique au robot Internet Archive de ne pas indexer un site:

```
User-agent: ia_archiver
Disallow: /
User-agent: archive.org_bot
Disallow: /
User-agent: ia_archiver-web.archive.org
Disallow: /
```

## Ressources

De bonnes infos fournies par Google: https://developers.google.com/search/docs/advanced/robots/create-robots-txt?hl=fr