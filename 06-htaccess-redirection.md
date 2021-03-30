---
layout: page
title: redirection htaccess
permalink: /redirection-htaccess/
---

Le fichier `.htaccess` permet également de mettre en place des redirections.

Voici une redirection qui s'applique au site entier:

```
Redirect 301 / https://example.com/
```

La redirection d'un fichier précis, vers un nouvel emplacement dans le même site:

```
Redirect /path/to/old/file/old.html /path/to/new/file/new.html
```

La redirection d'un fichier précis, vers un nouvel emplacement dans *un autre* site:

```
Redirect /path/to/old/file/old.html https://example.com/new/file/new.html
```

## Exemples: 

* [Les drapeaux de réécriture](https://httpd.apache.org/docs/current/rewrite/flags.html), Documentation Apache.
* [How can I redirect and rewrite my URLs with an .htaccess file?](https://help.dreamhost.com/hc/en-us/articles/215747748-How-can-I-redirect-and-rewrite-my-URLs-with-an-htaccess-file-), DreamHost Knowledge Base