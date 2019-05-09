## Main features

Yet, you may wonder why AnyBlok is useful for you?

[![Latest release on PyPI][pypi_anyblok_svg]][pypi_anyblok]
[AnyBlok][AnyBlok] takes advantages of famous python libraries and reuses
packages that already have proven their efficiency:

* Using [SQLAlchemy][sqlalchemy] AnyBlok is an [ORM][orm_wikipedia]:
  in few words you don't have to worry about the database management
  only your source code.
* With [Pyramid][pyramid_home] easily integrate any web protocol
  format, SOAP, JsonRPC, gRPC, REST.
* Integrated with [beaker][beaker] you'll be able to precisely manage
  browser sessions and cache.
* Using [dramatiq][dramatiq] you can easily scale your application and
  distribute asynchronously the workload through message brokers.
* And so on... Read more below about what values bloks have.

So you may use those libraries directly in your project but headaches
to make them working together by applying common patterns was already
spent by AnyBlok authors. Also it adds some nice features on its own:

* Work with multiple databases, to distribute your database workload
  you will be able to have one database to read/write and 3 others to
  read requests.
* Synchronize web session and SQL sessions
* Business API code: Business code is disconnected from any graphical
  user interface so it can be used with any of them. You may even
  integrate AnyBlok within existent python code.
* AnyBlok provide modularity mechanisms to let you easily reuse code
  distributed in different python packages.
* Make it easy to manage multiple levels of responsibilities in source code
  (for instance develop a generic product and manage
  customizations) by a dynamic inheritance mechanism.
* ...


We'll like to focus on [SqlAlchemy][sqlalchemy] to show AnyBlok's values:
[SqlAlchemy][sqlalchemy] is a powerful and well known ORM that let you
write python code and not worry about SQL. It also provides the required
abstractions so it can speak with different Database systems (PostgreSQL,
mariadb, SQLite...)

Great but what is the value of AnyBlok then? We could use Pyramid
and sqlAlchemy without AnyBlok if we only require a web service ?

Yes that's true! Here are some examples of AnyBlok values:

* It already does the hard work to make those great python libraries talk
  together.
* Load python module at runtime: you can add blok at runtime, so you
  don't have to stop the service to add functionalities.
* With the same python packages installed in one environment, you can
  get multiple instance with different behaviors according to which bloks
  you choose to install. This information is saved in the database so each
  database is an instance that can get a very different behaviours with only
  one running server.
* You can easily separate source code in different bloks, in a blok
  you will be able to extend, overwrite or overload other bloks,
  this allows you to customize all components of an application without
  doing any changes in a blok developed by someone else. You have
  full control over modules import order and bloks dependencies.

[AnyBlok]: https://github.com/AnyBlok/AnyBlok
[pypi_anyblok]: https://pypi.python.org/
[pypi_anyblok_svg]: https://img.shields.io/pypi/v/AnyBlok.svg
[beaker]: https://github.com/bbangert/beaker
[dramatiq]: https://dramatiq.io
[orm_wikipedia]: https://en.wikipedia.org/wiki/Object-relational_mapping
[pyramid_home]: https://trypyramid.com/pypi/AnyBlok
[sqlalchemy]: http://www.sqlalchemy.org/
