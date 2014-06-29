### Introduction

Pour demander une modification code original, il faut suivre
https://help.github.com/articles/fork-a-repo et https://help.github.com/articles/using-pull-requests

### Modifications du code source divio/djangocms-link

Il faut travailler sur une copie du projet
(Cf.https://help.github.com/articles/fork-a-repo).
À la page https://github.com/divio/djangocms-link on clique sur Fork.
Cela crée https://github.com/fp4code/djangocms-link

dans  $L/git/github.com/fp4code je clone :

```bash
git clone https://github.com/fp4code/djangocms-link.git
```

Je veux suivre le dépôt original
```bash
cd djangocms-link
git remote add upstream https://github.com/divio/djangocms-link.git
git fetch upstream
```

#### L'icône link.png n'apparait pas

C'est parce qu'elle est dans /static/cms/img/icons/plugins/link.png
et non pas dans /static/cms/images/plugins/link.png

Il faut modifier ./djangocms_link/cms_plugins.py

```bash
cd $L/git/github.com/fp4code/djangocms-link
git checkout -b link_image_directory
emacs djangocms_link/cms_plugins.py
git commit -am "changed cms/images/plugins to cms/img/icons/plugins"
git log -n 1 # -> commit d9b1b70
```

```bash
# pip install --force-reinstall git+file:$L/git/github.com/fp4code/djangocms-link@d9b1b70
pip uninstall djangocms-link
pip install git+file:$L/git/github.com/fp4code/djangocms-link@d9b1b70
cat env/lib/python2.7/site-packages/djangocms_link/cms_plugins.py # img/icons/plugins
```

Le lien apparait bien, mais seulement si on réédite plugins des pages puisque
l'ancienne adresse cms/images/plugins est codée en dur dans chaque page de texte.

Je suis satisfait de ma modification, je le mets sur le dépôt.

```bash
cd $L/git/github.com/fp4code/djangocms-link
git checkout master
git merge link_image_directory
git push origin master
```

À la page https://github.com/fp4code/djangocms-link je clique sur compare pour voir
la modification.

À la page https://github.com/divio/djangocms-link je clique sur Pull Requests.
