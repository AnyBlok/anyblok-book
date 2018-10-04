## Main features

Yet, you may wonder why AnyBlok is useful for you?

[![Latest release on PyPI][pypi_anyblok_svg]][pypi_anyblok]
[AnyBlok][AnyBlok] takes advantages of famous python libraries and reuse
packages that already have proven their existences:

* Using [SQLAlchemy][sqlalchemy] AnyBlok is an [ORM][orm_wikipedia]:
  in few words you don't have to worry about the database management
  only your code source.
* With [Pyramid][pyramid_home] easily integrate any web protocol
  format, SOAP, JsonRPC, gRPC, REST.
* Integrated with [beaker][beaker] you'll be able to precisely manage
  web sessions and cache.
* Using [dramatiq][dramatiq] you can easily scale your application and
  distribute the workload
* And so on... Read more about what values have bloks below 

So you may use those libraries directly in your project but headaches
to make them working together by applying common patterns was already
spent by AnyBlok authors. Also it adds some nice feature by its own:

* Work with multiple databases, to distribute your database workload
  you will be able to have one database for read/write and 3 others for
  read requests.
* Synchronize web session and SQL sessions
* Business API code: business code is disconnected from any graphical
  user interface so it can be use with any of them. You may even
  integrate AnyBlok within existent python code.
* AnyBlok provide modularity mechanisms to let you easily reuse code
  distributed in different python packages.
* Make it easy to manage multiple levels of responsibilities in code
  source (for instance develop a generic product and manage
  customizations) by a dynamic inheritance mechanism.
* ...


We'll like to focus on [SqlAlchemy][sqlalchemy] to show AnyBlok's values:
[SqlAlchemy][sqlalchemy] is a powerful and well known ORM that let you
write python code and not worry about SQL. It also provide the required
abstractions so it can speak with different Database systems (PostgreSQL,
mariadb, SQL Lite...)

Great but what is the value of AnyBlok then? We could use Pyramid
and sqlAlchemy without AnyBlok if we only require a web service ?

Yes that's true! here are some examples of AnyBlok values:

* It already do the work to make those great python libraries work
  together.
* load python module at runtime: you can add blok at runtime, so you
  don't have to stop the service to add functionalities.
* With the same python packages installed in one environment, you can
  get multiple instance with different behaviors according which bloks
  you have chosen to install. This information is saved in the
  database so each database is an instance that can get a very different
  behaviours with only one running server.
* You can easily separate code source in different bloks, in a blok
  you will be able to extend, overwrite or overload other bloks,
  this let you customize all components of an application without
  doing any change in blok developed by someone else. You have
  the full control to ordering module import and bloks dependencies.

[AnyBlok]: https://github.com/AnyBlok/AnyBlok
[pypi_anyblok]: https://pypi.python.org/
[pypi_anyblok_svg]: https://img.shields.io/pypi/v/AnyBlok.svg
[beaker]: https://github.com/bbangert/beaker
[dramatiq]: https://dramatiq.io
[orm_wikipedia]: https://en.wikipedia.org/wiki/Object-relational_mapping
[pyramid_home]: https://trypyramid.com/pypi/AnyBlok
[sqlalchemy]: http://www.sqlalchemy.org/
