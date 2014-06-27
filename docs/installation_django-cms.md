### Installation d'un environnement python 2.7.6 et de ipython

```bash
virtualenv env
source env/bin/activate
pip install /local/src/ipython-2.1.0.tar.gz
```

### Installation minimale de django-cms et de djangocms-installer.

```bash
cat >| install.sh <<EOF
#
pip install -e git+file:/local/git/github.com/django/django@1.6.5#egg=django
#
pip install -e git+file:/local/git/github.com/ojii/django-classy-tags@0.5.1#egg=django-classy-tags
pip install -e hg+file:/local/hg/bitbucket.org/andrewgodwin/south/@0.8.4#egg=south
pip install -e hg+file:/local/hg/bitbucket.org/gutworth/six/@1.7.2#egg=six
pip install -e git+file:/local/git/github.com/html5lib/html5lib-python@0.999#egg=html5lib-python
#
pip install -e git+file:/local/git/github.com/django-mptt/django-mptt@0.6.1#egg=django-mptt
pip install -e git+file:/local/git/github.com/ojii/django-sekizai@0.7#egg=django-sekizai
pip install -e git+file:/local/git/github.com/divio/djangocms-admin-style@0.2.2#egg=djangocms-admin-style
pip install -e git+file:/local/git/github.com/divio/django-cms@276fd3#egg=django-cms # 3.0.2+
pip install -e git+file:/local/git/github.com/psycopg/psycopg2@2_5_3#egg=psycopg2
#
pip install -e git+file:/local/git/github.com/kennethreitz/dj-database-url@v0.3.0#egg=dj-database-url
pip install -e git+file:/local/git/github.com/nephila/djangocms-installer@a837bb3eaecc#egg=djangocms-installer
#
pip install -e git+file:/local/git/github.com/python-pillow/Pillow@2.4.0#egg=Pillow
pip install -e git+file:/local/git/github.com/divio/djangocms-text-ckeditor@2.1.6#egg=djangocms-text-ckeditor
pip install -e git+file:/local/git/github.com/divio/djangocms-style@1.3#egg=djangocms-style
pip install -e git+file:/local/git/github.com/divio/djangocms-column@1.3#egg=djangocms-column
pip install -e git+file:/local/git/github.com/divio/djangocms-file@0.0.1#egg=djangocms-file
pip install -e git+file:/local/git/github.com/divio/djangocms-flash@0.0.2#egg=djangocms-flash
pip install -e git+file:/local/git/github.com/divio/djangocms-googlemap@0.0.5#egg=djangocms-googlemap
pip install -e git+file:/local/git/github.com/divio/djangocms-inherit@0.0.1#egg=djangocms-inherit
pip install -e git+file:/local/git/github.com/divio/djangocms-link@1.3.4#egg=djangocms-link
pip install -e git+file:/local/git/github.com/divio/djangocms-picture@0.0.2#egg=djangocms-picture
pip install -e git+file:/local/git/github.com/divio/djangocms-teaser@0.0.1#egg=djangocms-teaser
pip install -e git+file:/local/git/github.com/divio/djangocms-video@0.0.1#egg=djangocms-video
pip install -e git+file:/local/git/github.com/etianen/django-reversion@release-1.8#egg=django-reversion # 1.8.1 ne convient pas
#
EOF

bash install.sh
```

La compilation de Pillow a pu demander l'installation de bibliothèques graphiques.

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

### Base de données PostgreSQL

```bash

sudo su - postgres
#
B=base_django05
#
dropdb $B
dropuser $B
dropuser $USER
#
psql -U postgres template1 -c "CREATE ROLE $B NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT NOLOGIN;"
psql -U postgres template1 -c "CREATE ROLE $USER NOSUPERUSER NOCREATEDB NOCREATEROLE NOINHERIT LOGIN;"
psql -U postgres template1 -c "GRANT $B TO $USER;"
psql -U postgres template1 -c "CREATE DATABASE $B WITH OWNER=$USER;"
psql -U postgres template1 -c "REVOKE ALL ON DATABASE $B FROM public;"
psql -U postgres $B        -c "GRANT ALL ON SCHEMA public TO $USER WITH GRANT OPTION;"
#
logout
```

### création du site django-cms

```bash
rm -rf manage.py my_site static
djangocms -n -p . my_site --no-input --no-deps --db postgres:///base_django05 --cms-version 3.0 --django-version 1.6 --i18n yes \
--reversion no --languages fr,en --timezone Europe/Paris --use-tz yes --permissions yes --bootstrap yes --starting-page yes
```
