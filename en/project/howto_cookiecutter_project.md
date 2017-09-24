# Setup your own project

In this chapter you will learn how to start an Anyblok project ready to
run and hack.

## Project initialisation

We assume you have at least python3.3 environement ready to works with
and Postgresql running.

To quickly setup a new project we maintain [a cookiee cutter recipe]
[cookiecutter] which help you to generate an AnyBlok project while
awnsering to few awnsers:

```bash
$ pip install cookiecutter
$ cookiecutter gh:AnyBlok/cookiecutter-anyblok-project
project_name [Project name]: my_project
project_slug [my_project]: 
project_short_description [A short description of the Anyblok based project]: AnyBlok test
python_package [my_project]: 
blok_name [my_project]: blok1
Select db_driver_name:
1 - postgresql
Choose from 1 [1]: 
db_name [my_project]: 
Select http_server:
1 - no
2 - anyblok_pyramid
Choose from 1, 2 [1]: 2
Select open_source_license:
1 - Mozilla Public License Version 2.0
2 - GNU General Public License v3
3 - MIT license
4 - BSD license
5 - ISC license
6 - Apache Software License 2.0
7 - Not open source
Choose from 1, 2, 3, 4, 5, 6, 7 [1]: 
version [0.1.0]: 
full_name [Your name]: Jean-Sébastien Suzanne
email [Your address email (eq. you@example.com)]: jssuzanne@anybox.fr
github_username [Your github username]: jssuzanne

```

Congratulations! Your project is there with a blok ``blok1`` and
an *Example* model.


## Setup environement

We have a project directory but before running Anyblok you need
to get dependencies likes AnyBlok itself and setup a database:

```bash
$ cd my_project
make setup-dev
```


## Run AnyBlok

As long you choose ``anyblok_pyramid`` you have two options, open
a console interpreter to add *Example* record in your database or
start the development web server and access to configured urls.


### Console interpreter

This show how to add and request the Example model:

```python
$ anyblok_interpreter -c app.test.cfg

In [1]: registry.Example.query().all()
Out[1]: [<Example: An example, 1>]

In [2]: registry.Example.query().all().name
Out[2]: ['An example']

In [3]: registry.Example.insert(name="Another example")
Out[3]: <Example: Another example, 2>

In [4]: registry.Example.query().all()
Out[4]: [<Example: An example, 1>, <Example: Another example, 2>]

In [5]: registry.Example.query().all().name
Out[5]: ['An example', 'Another example']

In [7]: registry.commit()
```

### Run webserver

To run a development webserver execute:

```bash
$ make run-dev
anyblok_pyramid -c app.dev.cfg --wsgi-host 0.0.0.0
AnyBlok Load init: EntryPoint.parse('anyblok_pyramid_config = anyblok_pyramid:anyblok_init_config')
Load config file '/etc/xdg/AnyBlok/conf.cfg'
Load config file '/home/pverkest/.config/AnyBlok/conf.cfg'
Load config file '/home/pverkest/AnyBlok/cookiecutter-anyblok-project/travis_pyramid/app.dev.cfg'
Load config file '/home/pverkest/AnyBlok/cookiecutter-anyblok-project/travis_pyramid/app.cfg'
```

Then open your favorite web browser and visit following urls:

```bash
$ curl localhost:8080/
<a href="./example">List all availaible examples</a><br/>
<a href="./example/1">Get example id=1</a>

$ curl localhost:8080/example
[{"id": 1, "name": "An example"}]

$ curl localhost:8080/example/1
An example
```


## Conclusion

We have seen how to bootstrap an AnyBlok project without explain
what are each components if you want to go futher you may read
[project section](project/README.md)


[cookiecutter]: https://github.com/AnyBlok/cookiecutter-anyblok-project
