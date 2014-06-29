### Introduction

Installation d'un site django-cms minimal,
avec connexion à une base postgresql sans mot
de passe, au travers de socket unix.

### Variables d'environnement utilisées

#### sur portable de développement

```
export L=/local2 # répertoire qui contient git, hg et src. 
export B=base_django05 # base PostgreSQL qui sera utilisée
export R=/local2/django-cms/django05
export U=$USER
```

#### sur machine de test

```
export L=/local2 # répertoire qui contient git, hg et src. 
export B=base_django05 # base PostgreSQL qui sera utilisée
export U=django05
export R=/home/django05/django05
sudo -E su $U
```

### Installation d'un environnement python 2.7.6 et de ipython

```bash
cd $R
virtualenv env
source env/bin/activate
pip install $L/src/ipython-2.1.0.tar.gz
```

### Installation minimale de django-cms et de djangocms-installer.

```bash
cat >| install.sh <<EOF
#
pip install git+file:$L/git/github.com/django/django@1.6.5
#
pip install git+file:$L/git/github.com/ojii/django-classy-tags@0.5.1
pip install -e hg+file:$L/hg/bitbucket.org/andrewgodwin/south/@0.8.4#egg=south
pip install -e hg+file:$L/hg/bitbucket.org/gutworth/six/@1.7.2#egg=six
#pip install git+file:$L/git/github.com/html5lib/html5lib-python@0.999
pip install $L/src/html5lib-0.999.tar.gz
#
pip install git+file:$L/git/github.com/django-mptt/django-mptt@0.6.1
pip install git+file:$L/git/github.com/ojii/django-sekizai@0.7
pip install git+file:$L/git/github.com/divio/djangocms-admin-style@0.2.2
pip install git+file:$L/git/github.com/divio/django-cms@276fd3 # 3.0.2+
pip install git+file:$L/git/github.com/psycopg/psycopg2@2_5_3
#
pip install git+file:$L/git/github.com/kennethreitz/dj-database-url@v0.3.0
pip install git+file:$L/git/github.com/nephila/djangocms-installer@a837bb3
#
pip install git+file:$L/git/github.com/python-pillow/Pillow@2.4.0
pip install git+file:$L/git/github.com/divio/djangocms-text-ckeditor@2.1.6
pip install git+file:$L/git/github.com/divio/djangocms-style@1.3
pip install git+file:$L/git/github.com/divio/djangocms-column@1.3
pip install git+file:$L/git/github.com/divio/djangocms-file@0.0.1
pip install git+file:$L/git/github.com/divio/djangocms-flash@0.0.2
pip install git+file:$L/git/github.com/divio/djangocms-googlemap@0.0.5
pip install git+file:$L/git/github.com/divio/djangocms-inherit@0.0.1
pip install git+file:$L/git/github.com/divio/djangocms-link@1.3.4
pip install git+file:$L/git/github.com/divio/djangocms-picture@0.0.2
pip install git+file:$L/git/github.com/divio/djangocms-teaser@0.0.1
pip install git+file:$L/git/github.com/divio/djangocms-video@0.0.1
pip install git+file:$L/git/github.com/etianen/django-reversion@release-1.8 # 1.8.1 ne convient pas
#
EOF

bash install.sh
```

#### NOTES

La compilation de Pillow demande l'installation préalable de plusieurs bibliothèques graphiques standards.

```
    --------------------------------------------------------------------
    PIL SETUP SUMMARY
    --------------------------------------------------------------------
    version      Pillow 2.4.0
    platform     linux2 2.7.6 (default, Mar 22 2014, 22:59:56)
                 [GCC 4.8.2]
    --------------------------------------------------------------------
    --- TKINTER support available
    --- JPEG support available
    *** OPENJPEG (JPEG2000) support not available
    --- ZLIB (PNG/ZIP) support available
    --- LIBTIFF support available
    --- FREETYPE2 support available
    --- LITTLECMS2 support available
    --- WEBP support available
    --- WEBPMUX support available
    --------------------------------------------------------------------
```

L'option -e de `pip install` donne une configuration d'installation
(visible avec `pip freeze`) qui rend invisibles html5lib et psycopg2.

Pour `git install -e hg+...`, l'option -e semble indispensable.

L'installation de html5lib à partir de git est problématique.

### Base de données PostgreSQL

```bash
sudo -E su postgres # -E pour transmettre B et U
#
dropdb $B
dropuser $B
dropuser $U
#
psql -U postgres template1 -c "CREATE ROLE $B NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT NOLOGIN;"
psql -U postgres template1 -c "CREATE ROLE $U NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;"
psql -U postgres template1 -c "GRANT $B TO $U;"
psql -U postgres template1 -c "CREATE DATABASE $B WITH OWNER=$U;"
psql -U postgres template1 -c "REVOKE ALL ON DATABASE $B FROM public;"
psql -U postgres $B        -c "GRANT ALL ON SCHEMA public TO $U WITH GRANT OPTION;"
#
exit
```

### création du site django-cms

```bash
rm -rf manage.py my_site static
djangocms -n -p . my_site --no-input --no-deps --db postgres:///$B --cms-version 3.0 --django-version 1.6 --i18n yes \
--reversion no --languages fr,en --timezone Europe/Paris --use-tz yes --permissions yes --bootstrap yes --starting-page yes
```

On répond
```
Creating admin user
Username: admin
Email address: 
Password: XXX_SECRET_XXX
Password (again): LE_MEME
Superuser created successfully.
All done!
```

### copie des css et js jquery et bootstrap

Il y a peut-être mieux pour récupérer les fichiers dans une version donnée.

```bash
cd $L/git/github.com/jquery/jquery
git checkout 2.1.1
cd $L/git/github.com/twbs/bootstrap
git checkout v3.2.0
```

```bash
mkdir -p my_site/static/bootstrap/3.0.3
cp -rp $L/git/github.com/twbs/bootstrap/dist/* my_site/static/bootstrap/3.0.3
#
mkdir -p my_site/static/jquery/2.1.1
cp -rp $L/git/github.com/jquery/jquery/dist/* my_site/static/jquery/2.1.1
```

```bash
./manage.py collectstatic
```

on répond `yes`

```bash
sed -i 's@//netdna.bootstrapcdn.com/@/static/@g' my_site/templates/base.html 
sed -i 's@http://code.jquery.com/@/static/jquery/2.1.1/@g' my_site/templates/base.html 
```

Lancement du serveur

```bash
python manage.py runserver 8005
```

On a le site à l'adresse http://localhost:8005/

### Interface pour le serveur nginx

```bash
pip install  git+file:/local2/git/github.com/benoitc/gunicorn@19.0
```


### Ajout d'applications utiles

```bash
pip install git+file:/local2/git/github.com/SmileyChris/easy-thumbnails@2.0.1
pip install git+file:/local2/git/github.com/chrisglass/django_polymorphic@v0.5.5
pip install git+file:/local2/git/github.com/jezdez/django-appconf@v0.6
pip install git+file:/local2/git/github.com/stefanfoulis/django-filer@0.9.6
pip install git+file:/local2/git/github.com/stefanfoulis/cmsplugin-filer@0.9.8
```

Ajout à settings.py:
```
INSTALLED_APPS = (
    ...
    'filer',
    'easy_thumbnails',
    ...
)

...

SOUTH_MIGRATION_MODULES = {
    'easy_thumbnails': 'easy_thumbnails.south_migrations',
}

```

```bash
./manage.py syncdb --migrate