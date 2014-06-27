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

> Lorem ipsum dolor sit amet, consectetur adipisicing elit,
> sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
> Ut enim ad minim veniam,
> quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat.
> Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
> Excepteur sint occaecat cupidatat non proident,
> sunt in culpa qui officia deserunt mollit anim id est laborum

Un *texte en italique* et _un autre aussi_.
Un **texte en gras** et __un autre aussi__.

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

On utilise l'apostrophe inverse pour une `police code fixe`.

On la triple pour un ensemble de lignes

```
import numpy as np
a = np.arange(10)
def foo:
  if not bar:
    return true
```

On peut rendre ces lignes de code plus belles

```python
import numpy as np
a = np.arange(10)
def foo:
  if not bar:
    return true
```



La syntaxe des liens c'est crochets puis parenthèses.
Ainsi pour la [racine de ce dépôt](https://github.com/fp4code/lpnupr20migration).

