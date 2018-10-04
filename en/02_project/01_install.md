## Anyblok Installation


### Requirements

All this book was tested using PostgreSQL so we recommend to install it locally.

To develop an AnyBlok project you need to make sure you have
python >= 3.3 and a SGDB supported by [SQLAlchemy]
[sqlalchemy_requirements]

[comment]: <> (TODO: make sure reader has the expected role in its database)

### Virtual environment

We advice to use a [python virtual environment](
https://docs.python.org/3/tutorial/venv.html). One of its main advantage
is to make you able to develop different projects with different
version of libraries. This avoid conflict when 2 applications depend on
different versions of the same library.

Here is how to create the virtualenv and activating it.

```bash
python3 -m venv rooms-venv
source rooms-venv/bin/activate
```
Once activated all python commands (likes ``pip`` and so on) are launched in
this context.

In all this book we assume you have activated your virtual environment. You can
make sure of it by looking at the left of your terminal, 
its name is generally displayed.

```bash
(rooms-venv) yourname@mycomputer:~/anyblok$ 
```


[sqlalchemy_requirements]: http://docs.sqlalchemy.org/en/latest
