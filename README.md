# blog-posts
Makina corpus blogposts

## écrire en markdown, publier sur plone

Différentes solutions ont été proposées pour poster les articles sur plone, notamment pour le support de la coloration syntaxique :

### publier en rst

En utilisant `pandoc`, rien de plus simple :

```
pandoc design-api-pour-app-mobile.md -o design-api-pour-app-mobile.rst
```

### hoster le code dans des gist

Et dans ce cas utiliser des iframes pour afficher les exemples de code.
