## Anyblok Installation


### Requirements

All this book was test using PostgreSQL so we recommends to install it locally.

To develop an AnyBlok project you needs to make sure you have
python >= 3.3 and a SGDB supported by [SQLAlchemy]
[sqlalchemy_requirements]

[comment]: <> (TODO: make sure reader has the expected role in its database)

### Virtual environment

We advice to use a [python virtual environment](
https://docs.python.org/3/tutorial/venv.html) one of it's main advantage
is to give the capabilities to develop different projects with different
version of libraries. This avoid conflict while 2 applications depends on
different versions of the same library.

Here how to create the virtualenv and activated.

```bash
python3 -m venv rooms-venv
source rooms-venv/bin/activate
```
Once activated all python commands (likes ``pip`` and so on) are launched in
this context.

In all this book we assume you have activated your virtual environment you can
see it because its name is generaly shown on the left of your terminal

```bash
(rooms-venv) yourname@mycomputer:~/anyblok$ 
```


[sqlalchemy_requirements]: http://docs.sqlalchemy.org/en/latest
