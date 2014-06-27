Le format GFM des fichiers de documentation dans github
est décrit ici :
https://help.github.com/articles/github-flavored-markdown
avec les bases expliquées là :
https://help.github.com/articles/markdown-basics

Une ligne blanche sert à séparer les paragaphes.

Les niveaux de titre de rubriques sont déterminés
par le nombre de signes #

# Titre niveau 1
## Titre niveau 2
### Titre niveau 3

Les lignes d'un bloc de citation sont précédées de >

> Première ligne
> Deuxième ligne

Un *texte en gras* et un **autre en italique**
ainsi que _la combinaison des deux_.

Une liste

* Item
* Item
* Item

Ou encore

- Item
- Item
- Item

Pour numéroter les items

1. Item 1
2. Item 2
  1. Item 2.2 débute avec deux espaces
    * sous-liste de niveau 3 avec 4 espaces
    * etc.
3. Item 3

On utilise l'apostrophe inverse pour une `police fixe`.

On la triple pour un ensemble de lignes

```
import numpy as np
a = np.arange(10)
```

La syntaxe des liens c'est crochets puis parenthèses.
Ainsi pour la [racine de ce dépôt](https://github.com/fp4code/lpnupr20migration).



