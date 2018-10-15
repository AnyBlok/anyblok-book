## Anyblok Installation


### Requirements

To develop an AnyBlok project you need to make sure you have
python >= 3.3 and a SGDB supported by [SQLAlchemy]
[sqlalchemy_requirements]. Also, don't forget to install [Git][git]

All this book was tested using Debian and PostgreSQL. Here is how to install all
system dependencies on Debian like operating system:

```bash
sudo apt-get install postgresql libpq-dev
sudo apt-get install python3-dev python3-venv
sudo apt-get install git libffi-dev libssl-dev libxml2-dev libxslt1-dev libyaml-dev zlib1g-dev
```

Once your operating system and database engine are ready, it's time to create
a Postgresql user for the project (Replace 'myusername' with yours).

```bash
sudo -u postgres createuser --createdb --createrole --superuser myusername
```

### Virtual environment

We advice to use a [python virtual environment](
https://docs.python.org/3/tutorial/venv.html). One of its main advantage
is to make you able to develop different projects with different
versions of libraries. This avoid conflict when 2 applications depend on
different versions of the same library.

Here is how to create the virtualenv and activate it.

```bash
python3 -m venv rooms-venv
source rooms-venv/bin/activate
```

Once activated, all python commands (like ``pip`` and so on) are launched in
this context.

In all this book we assume you have activated your virtual environment. You can
make sure of it by looking at the left of your terminal, 
its name is generally displayed.

```bash
(rooms-venv) yourname@mycomputer:~/anyblok$ 
```

[sqlalchemy_requirements]: http://docs.sqlalchemy.org/en/latest
[git]: https://git-scm.com/
