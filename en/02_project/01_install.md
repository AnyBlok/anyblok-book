## Anyblok Installation


### Requirements

To develop an AnyBlok project you need to make sure you have
python >= 3.6 and a SGDB supported by [SQLAlchemy]
[sqlalchemy_requirements]. Also, don't forget to install [Git][git]

All this book was tested using Debian and PostgreSQL. Here is how to install all
system dependencies on a Debian like operating system:

```bash
sudo apt-get install postgresql libpq-dev
sudo apt-get install python3-dev python3-venv
sudo apt-get install git libffi-dev libssl-dev libxml2-dev libxslt1-dev libyaml-dev zlib1g-dev
```


Once your operating system and database engine are ready, it's time to create
a Postgresql role for the project (Replace 'myusername') that matches
your host user, at least you should be able to createdb/dropdb without errors:

```bash
createdb testws
dropdb testws
```

If you get some errors create a Postgresql role (it's up to you, you can
set yourself as superuser at the same time):

```bash
sudo -u postgres createuser --createdb --superuser myusername
```

### Virtual environment

We advice to use a [python virtual environment](
https://docs.python.org/3/tutorial/venv.html). One of its main features
is to make you able to develop different projects with different
versions of libraries. This avoid conflict when 2 applications are depending on
different versions of the same library.

Here is how to create the virtualenv and activate it.

```bash
python3 -m venv rooms-venv
source rooms-venv/bin/activate
```

Once activated, all python commands (like ``pip`` and so on) are launched in
this context.

In all book examples, we assume you have activated your virtual environment.
Make sure it is activated by looking at the left part of your terminal,
its name is generally displayed.

```bash
(rooms-venv) yourname@mycomputer:~/anyblok$ 
```

[sqlalchemy_requirements]: http://docs.sqlalchemy.org/en/latest
[git]: https://git-scm.com/
