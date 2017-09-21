# Installer AnyBlok 

AnyBlok est un framework python. Il utilise l'ORM SQLAlchemy. Pour les besoins du tutoriel nous allons utiliser Postgresql comme serveur de base de données. Ce choix est lié uniquement au fait que je maîtrise davantage cette BDD que les autres. Si vous souhaitez utiliser une autre BDD, vous devrez adapter les parties installation et l'option de commande --db-driver-name. 


## Préparer votre instance de travail.

Dans un premier temps installer votre serveur de base de donnée préféré.

> sudo apt-get install postgresql

Pour plus d'informations, vous pouvez suivre les indications des documentations suivantes:
* [Ubuntu](https://doc.ubuntu-fr.org/postgresql)
* [Debian](https://wiki.debian.org/PostgreSql)
* [Fedora](http://doc.fedora-fr.org/wiki/Installation_et_configuration_de_PostgreSQL)
* [Mac OS avec macport](https://coderwall.com/p/xezzaa/install-postgresql-9-2-on-os-x-mountain-lion)

Faite un environnement virtuel afin d'isoler les librairies utilisées de celle de votre system.

```
pyvenv-3.5 sandbox
```


## Installer anyblok

AnyBlok est disponible sur le **pypi**, il suffit donc de l'installé avec la commande pip

```
sandbox/bin/pip install anyblok
```


## Installer une base de donnée.

Nous utilisons psycopg2 pour nous connecter a Postgres.

```
sandbox/bin/pip install psycopg2
```


## Créer une base

Anyblok fournit un script pour crée une base de donnée et l'initialiser. 

```
sandbox/bin/anyblok_createdb --db-name todolist --db-driver-name postgresql
==> Load config file '/Library/Application Support/AnyBlok/conf.cfg'
==> Load config file '/Users/jssuzanne/Library/Application Support/AnyBlok/conf.cfg'
```


## Lancer un interpreteur python.

L'interpreteur permet de se connecter a la base créé et d'utilisé l'api du serveur.
Il est util en developpement, déboggage ou simplement pour modifier la base.

```
sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql
==> Load config file '/Library/Application Support/AnyBlok/conf.cfg'
==> Load config file '/Users/jssuzanne/Library/Application Support/AnyBlok/conf.cfg'
==> >>>
```

Félicitation vous avez une première base AnyBlok d'installée. Il maintenant temps
de jouer avec et de l'enrichir.


## Lister les bloks disponibles

L'ORM étant celui de SQLAlchemy, les amoureux de cette librairie ne seront pas descus, Il existe cependant des différences. La session de SQLAlchemy est wrappé par AnyBlok.

Il faut donc remplacer:

```python
session.query(Foo)
```

par :

```python
registry.Foo.query()
```

Dans notre premier example nous allons simplement listé les bloks existant.

```
sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql
==> Load config file '/Library/Application Support/AnyBlok/conf.cfg'
==> Load config file '/Users/jssuzanne/Library/Application Support/AnyBlok/conf.cfg'
==> >>> registry.System.Blok.query().all()
==> >>> [anyblok-core (installed), anyblok-io (uninstalled), anyblok-io-csv (uninstalled), anyblok-io-xml (uninstalled), model_authz (uninstalled)]
```

## Pour les amoureux de IPython (Bonus)

Commencé par installé la librairie ipython

```
sandbox/bin/pip install ipython
```

L'interpreteur utilise directement ipython

```
sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql
==> Load config file '/Library/Application Support/AnyBlok/conf.cfg'
==> Load config file '/Users/jssuzanne/Library/Application Support/AnyBlok/conf.cfg'
==> In [1]: registry.System.Blok.query().all()
==> Out[1]:
==> [anyblok-core (installed),
==>  anyblok-io (uninstalled),
==>  anyblok-io-csv (uninstalled),
==>  anyblok-io-xml (uninstalled),
==>  model_authz (uninstalled)]
```
