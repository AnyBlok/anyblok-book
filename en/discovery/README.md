# Understand AnyBlok

In this chapter (I hope) you'll start to understand what's AnyBlok, mains
features, the current ecosystem. How it differ from others products.
Motivations and historicly why AnyBlok was created, Our values and why you
should use AnyBlok.

## What is it?

[AnyBlok][AnyBlok] is a Python framework allowing to create highly
dynamic and modular applications on top of the SQLAlchemy ORM.
Applications are made of “bloks” that can be installed, extended,
replaced, upgraded or uninstalled.

Bloks can provide SQL Models, Column types, Fields, Mixins, SQL views,
or even plain Python code unrelated to the database, and all of these
can be dynamically customized, modified, or extended without strong
dependencies between them, just by adding new bloks.


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
* Using [dramatiq][dramatiq] you can ealsy scale your application and
  distributed the workload
* And so on... Read more about existing bloks bellow

So you may use those librairies directly in your project but headaches
to make them working together by applying common patterns was already
spent by AnyBlok authors. Also it adds some nice feature by its own:

* Work with multiple database, to distribute your database workload
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


I'll like to focus on [SqlAlchemy][sqlalchemy] to show AnyBlok's values:
[SqlAlchemy][sqlalchemy] is a power full and well known ORM that let you
writing python code and not worry about SQL. It also provide the requiered
abstractions so it can speak with different Database system (PostgreSQL,
mariadb, SQL Lite...)

Great but what is the value of AnyBlok then? I could use Pyramid
and sqlAlchemy without AnyBlok if I only require a web service ?

Yes that true! here are some response of AnyBlok values:

* It already do the work to make those greate python libraries works
  together.
* load python module at runtime: You can add blok at runtime, so you
  don't have to stop the service to add functionalities.
* With sames python packages installed in one environement, you can
  get multiple instance with different behaviors according which bloks
  you have chosen to install. This information is saved in the
  database so each database is an instance that can get a very different
  behaviours with only one running server.
* You can easly separate code source in different bloks, in a blok
  you will be able to extend, overwrite or overload other bloks,
  this let you customize all components of an application without
  doing any change in blok develop by someone else. You have
  the full control to ordering module import and bloks dependencies.


## AnyBlok Ecosystem


> **Important**: We belives splitting business code and graphical user
> interface is an important effort that have to be done as web
> framework involve faster than backend business code that why we
> would like to create business APIs.


To illustrate components let's start with our vision about AnyBlok
ecosystem we would like to create and then we will deep in details with
AnyBlok components: existing bloks.

> **Note**: Keep in mind some of those features are forecast at the
> time I'm writing those lines.

```
+-----------------------------------------------------
| Furet UI
+----------------+------------------------------------
| Business Bloks UI
+----------------+------------------------------------
|                | AnyBlok Furet UI
+ Business Bloks +------------------------------------
|                | AnyBlok pyramid beaker
+----------+     +-----------------+--------------+-----------------------+-
| REA Blok |     | AnyBlok Pyramid | AnyBlok auth | AnyBlok multi engines | 
+----------+-----+-----------------+--------------+-----------------------+-
| AnyBlok
+-----------------------------------------------------
| SQL Alchemy
+-----------------------------------------------------
| SGBD: PostgreSQL / mariadb / SQL Lite / ...
+-----------------------------------------------------
```

As most ERP Product / business product we would like to create an ecosystem
around [AnyBlok][AnyBlok], at the time I'm writting those lines there is
only few business bloks written. Also we believe a big effort must be done
in tools rather than start writting business code. We wants to give to
developers a strong and stable tools to help them to wirte business bloks.

We think business applications as a service so business rules
should be disconnected from the graphical user interface, we would
like to create some kind of business API that let developer free to use
common business bloks as he wants:

  - within its own existing base code
  - expose web services over http
  - let choice to connect to any kind of interfaces (Graphical or not)


### Technical Bloks

[Furet UI][furetui] [POC not released] is web user interface
framework it can be use without AnyBlok as long the server implement
required interfaces (web services). The Idea of this UI framework is
that behaviors and representations are managed by server responses.
Then developers do not require to write javascript to manage his
business application. Developer focus on writing backend code (the
business) by providing expected data. FureUI use famous web framework
likes [Vue.js][vuejs] to manage the the html page, [bulma][bulma] a
powerfull css framework.

[![Latest release on PyPI][pypi_anyblok_furetui_svg]][pypi_anyblok_furetui]
[AnyBlok FuretUI][anyblok_furetui] is a *Furet UI* backend that helps to
generate graphical web interface to AnyBlok it gives features likes:

* overload graphical interfaces between bloks:

  Likes with AnyBlok where you are able to overload any python bloks
  AnyBlok FuretUI let you overload the user interface:

  for instance let say you are using those bloks ``product`` and
  ``product_furet_ui`` wich would be develop by someone
  else in the community that manage some basic product information (
  name, code, size, price) the other blok (``product_furet_ui``)
  manage how to display products in the graphical user interface.

  Imagine you want to extend the ``product_furet_ui`` functionality by
  adding EAN13 (barcode value). We would give you the following advice:
  create 2 bloks the first ``product_barcode`` will add fields and
  business methods to manage barecode on products. The second blok
  ``product_barecode_ui`` will extend the user interface defined in
  the ``product_furet_ui`` by adding the barcode field in the interface
  in the kind of way that tell:
  "hey! I would like to add the field 'barcode' just right after
  the existing field product code"

  Here an example of dependency tree:

  ```
  +-----------------------------------------------------
  | Furet UI
  +----------------+------------------------------------
  | Product barecode UI
  +----------------+------------------------------------
  |    Product     | AnyBlok Furet UI
  +                +------------------------------------
  |    Barecode    | AnyBlok pyramid beaker
  +----------+-----+-----------------+--------------+-----------------------+-
  | Product  | ... | AnyBlok Pyramid | AnyBlok auth | AnyBlok multi engines | 
  +----------+-----+-----------------+--------------+-----------------------+-
  | AnyBlok
  +-----------------------------------------------------
  | SQL Alchemy
  +-----------------------------------------------------
  | SGBD: PostgreSQL / mariadb / SQL Lite / ...
  +-----------------------------------------------------
  ```

[![Latest release on PyPI][pypi_anyblok_beaker_svg]][pypi_anyblok_beaker]
[AnyBlok Pyramid beaker][anyblok_beaker] using the strength of
[beaker][beaker] you are able to precisely configure web sessions and cache
on top of [pyramid][pyramid_home].


[![Latest release on PyPI][pypi_anyblok-pyramid-rest-api-svg]
][pypi_anyblok-pyramid-rest-api]
[AnyBlok Pyramid rest api][AnyBlok-pyramid-rest-api] this blok provide
facilities for building restful api that interacts with AnyBlok models
through a CRUD like pattern. It is based on [Cornice][cornice] that provides
helpers to build & document REST-ish Web Services with [Pyramid][pyramid_home],
with decent default behaviors. 

[![Latest release on PyPI][pypi_anyblok_bus_svg]][pypi_anyblok_bus]
[AnyBlok bus][anyblok_bus] Lets AnyBlok send/receive messages on a BUS. Based
on [Pika](https://pika.readthedocs.io/en/stable/) Anybloks can communicate
through the well known [AMQP](https://www.amqp.org/). Also based on 
[marshmallow][marshmallow] models are easly serialized/de-serialized.

[![Latest release on PyPI][pypi_anyblok_jsonschema_svg]][pypi_anyblok_jsonschema]
[AnyBlok jsonschema][anyblok_jsonschema] This bloks generate
[JSON Schema Draft v4](http://json-schema.org/) formatting from models using
[marshmallow-jsonschema](https://github.com/fuhrysteve/marshmallow-jsonschema)

[![Latest release on PyPI][pypi_anyblok_marshmallow_svg]][pypi_anyblok_marshmallow] 
[AnyBlok Marshmallow][anyblok_marshmallow] Add validator, serializer and
deserializer schema using [marshmallow][marshmallow]

[![Latest release on PyPI][pypi_anyblok_pyramid_svg]][pypi_anyblok_pyramid]
[AnyBlok Pyramid][anyblok_pyramid] allow to easly expose web interfaces
with the power of the well known [pyramid framework][pyramid_home].
The main benefits is that it synchronized the web session with the
sql session to the developer do not have to worries about commits,
rollbacks, manage pool of sql connections and so on.

[![Latest release on PyPI][pypi_anyblok_attachment_jinja_svg]
][pypi_anyblok_attachment_jinja]
[AnyBlok attachment jinja][anyblok_attachment_jinja] Allow jinja template
in your reports.

[![Latest release on PyPI][pypi_anyblok_attachment_svg]][pypi_anyblok_attachment]
[AnyBlok attachment][anyblok_attachment] The base blok to manage
attachement and report systems.

[![Latest release on PyPI][pypi_anyblok_postgres_svg]][pypi_anyblok_postgres]
[AnyBlok postgres][anyblok_postgres] add special features to AnyBlok that are dedicated and based on 
[PostgreSQL][postgresql] features. Bloks (will) exploit the strengh of:

* [JSONB field](
  https://www.postgresql.org/docs/current/static/datatype-json.html) wich add
  ``Jsonb`` column feature to AnyBlok
* [Materialized view](
  https://www.postgresql.org/docs/10/static/sql-creatematerializedview.html)
* ...

[![Latest release on PyPI][pypi_anyblok_io_svg]][pypi_anyblok_io]
[AnyBlok io][anyblok_io] Provide bloks regardings Input/Output (I/O) used
for import / export which allow to initialize data from files using any
formats (xml, csv, ...).

[![Latest release on PyPI][pypi_anyblok_mixins_svg]][pypi_anyblok_mixins]
[AnyBlok mixins][anyblok_mixins] 
Provide handy [mixins](https://en.wikipedia.org/wiki/Mixin) to speed up your
development, some features are:
* Make a model readonly
* Make a record readonly according some conditions
* Add common columns (likes id / create date / update date)
* Create workflow
* ...

[![Latest release on PyPI][pypi_anyblok_dramatiq_svg]][pypi_anyblok_dramatiq]
[AnyBlok dramatiq][anyblok_dramatiq]: [Dramatiq][dramatiq] is a distributed
task queueing libraray. This Blok makes facilities to use [Dramatiq][dramatiq]
with AnyBlok and distributed your workloads over multiple servers.

[![Latest release on PyPI][pypi_AnyBlok_Multi_Engines_svg]
][pypi_AnyBlok_Multi_Engines]
[AnyBlok Multi Engines][AnyBlok_Multi_Engines] This blok allow to use
multi database backends. Let's says you have one master with multiple
replicates database, you can easly send read request on replicates and
writes to the master.

### Business Bloks

[![Latest release on PyPI][pypi_anyblok_sale_svg]][pypi_anyblok_sale]
[AnyBlok sale][anyblok_sale] These are bloks used for sale managment.


[![Latest release on PyPI][pypi_anyblok_address_svg]][pypi_anyblok_address]
[AnyBlok address][anyblok_address] These are bloks for address managment.

[![Latest release on PyPI][pypi_anyblok_product_svg]][pypi_anyblok_product]
[AnyBlok Product][anyblok_product] Product management bloks.

[![Latest release on PyPI][pypi_anyblok_wms_base_svg]][pypi_anyblok_wms_base]
[AnyBlok WMS Base][anyblok_wms_base] These are base Bloks to build
Warehouse Management and Logistics applications with AnyBlok.

[![Latest release on PyPI][pypi_anyblok_rea_svg]][pypi_anyblok_rea]
[AnyBlok REA][anyblok_rea] give abstract class that you can apply
to your business, [REA][rea] itself is a collection of business
patern which you can apply to manage stock, accounts and so on...

I'm writing those lines in the early age of the ecosystem,
when ecosystem will grow it will gains much value.
We are working on that way adding bloks to manage common used cases.
Feel free to contribute, open issues, create pull request,
ask new repository through [github][gh_anyblok].

## AnyBlok versus others

You may wonder why use [AnyBlok][AnyBlok] compared with other existing
solutions.

### Odoo

AnyBlok mechanisms are fairly inspired from odoo. Here some
differences:

* AnyBlok is 100% OpenSource
* AnyBlok do not create a new ORM it use an existing powerful one for
  years: [SqlAlchemy][sqlalchemy]
* With AnyBlok you can use normal python packages to deliver and manage
  your code
* Backend and graphical user interface are split.
* With AnyBlok you can manage web sessions as you want
* AnyBlok reuse existing community packages as soon as it can

### SqlAlchemy / Pyramid

Some kind of things that you don't have to worry with AnyBlok that you should
implement if you use SQLAlchemy and  Pyramid by yourself:

* AnyBlok make easy to extend models betweens differents python packages
* AnyBlok allow to install different bloks provide by same packages to get
  differnts behaviours and model installed in the database
* Pyramid web session is synced with database transaction

But moving simple AnyBlok application code to pure SqlAlchemy wouldn't be
that hard.



[orm_wikipedia]: https://en.wikipedia.org/wiki/Object-relational_mapping
[beaker]: https://github.com/bbangert/beaker
[rea]: https://en.wikipedia.org/wiki/Resources,_events,_agents_(accounting_model)
[sqlalchemy]: http://www.sqlalchemy.org/
[pyramid_home]: https://trypyramid.com/
[gh_anyblok]: https://github.com/AnyBlok
[anyblok_furetui]: https://github.com/AnyBlok/anyblok_furetui
[pypi_anyblok_furetui]: https://pypi.python.org/pypi/anyblok_furetui
[pypi_anyblok_furetui_svg]: https://img.shields.io/pypi/v/anyblok_furetui.svg
[anyblok_pyramid]: https://github.com/AnyBlok/anyblok_pyramid
[pypi_anyblok_pyramid]: https://pypi.python.org/pypi/anyblok_pyramid
[pypi_anyblok_pyramid_svg]: https://img.shields.io/pypi/v/Anyblok_Pyramid.svg
[anyblok_beaker]: https://github.com/AnyBlok/AnyBlok_Pyramid_Beaker
[pypi_anyblok_beaker]: https://pypi.python.org/pypi/AnyBlok_Pyramid_Beaker
[pypi_anyblok_beaker_svg]: https://img.shields.io/pypi/v/anyblok_pyramid_beaker.svg
[anyblok_rea]: https://github.com/AnyBlok/anyblok_rea
[pypi_anyblok_rea]: https://pypi.python.org/pypi/anyblok_rea
[pypi_anyblok_rea_svg]: https://img.shields.io/pypi/v/anyblok_rea.svg
[anyblok_wms_base]: https://pypi.python.org/pypi/anyblok_wms_base
[pypi_anyblok_wms_base]: https://pypi.org/project/anyblok_wms_base
[pypi_anyblok_wms_base_svg]: https://img.shields.io/pypi/v/anyblok_wms_base.svg
[furetui]: https://github.com/AnyBlok/furet_ui
[vuejs]: https://vuejs.org/
[bulma]: http://bulma.io/
[anyblok_product]: https://github.com/AnyBlok/anyblok_product
[pypi_anyblok_product]: https://pypi.org/project/anyblok_product
[pypi_anyblok_product_svg]: https://img.shields.io/pypi/v/anyblok_product.svg
[anyblok_marshmallow]: https://github.com/AnyBlok/AnyBlok_Marshmallow
[pypi_anyblok_marshmallow]: https://pypi.python.org/pypi/AnyBlok_Marshmallow
[pypi_anyblok_marshmallow_svg]: https://img.shields.io/pypi/v/anyblok_marshmallow.svg
[marshmallow]: https://marshmallow.readthedocs.io/en/latest/
[cornice]: https://cornice.readthedocs.io/en/latest/
[AnyBlok-pyramid-rest-api]: https://github.com/AnyBlok/AnyBlok-pyramid-rest-api
[pypi_anyblok-pyramid-rest-api]: https://pypi.python.org/pypi/AnyBlok-pyramid-rest-api
[pypi_anyblok-pyramid-rest-api-svg]: https://img.shields.io/pypi/v/AnyBlok-pyramid-rest-api.svg
[anyblok_attachment]: https://github.com/AnyBlok/anyblok_attachment
[pypi_anyblok_attachment]: https://pypi.python.org/pypi/anyblok_attachment
[pypi_anyblok_attachment_svg]: https://img.shields.io/pypi/v/anyblok_attachment.svg
[anyblok_attachment_jinja]: https://github.com/AnyBlok/anyblok_attachment_jinja
[pypi_anyblok_attachment_jinja]: https://pypi.python.org/pypi/anyblok_attachment_jinja
[pypi_anyblok_attachment_jinja_svg]: https://img.shields.io/pypi/v/anyblok_attachment_jinja.svg
[anyblok_postgres]: https://github.com/AnyBlok/anyblok_postgres
[pypi_anyblok_postgres]: https://pypi.python.org/pypi/anyblok_postgres
[pypi_anyblok_postgres_svg]: https://img.shields.io/pypi/v/anyblok_postgres.svg
[anyblok_io]: https://github.com/AnyBlok/anyblok_io
[pypi_anyblok_io]: https://pypi.python.org/pypi/anyblok_io
[pypi_anyblok_io_svg]: https://img.shields.io/pypi/v/anyblok_io.svg
[anyblok_mixins]: https://github.com/AnyBlok/anyblok_mixins
[pypi_anyblok_mixins]: https://pypi.python.org/pypi/anyblok_mixins
[pypi_anyblok_mixins_svg]: https://img.shields.io/pypi/v/anyblok_mixins.svg
[anyblok_dramatiq]: https://github.com/AnyBlok/anyblok_dramatiq
[pypi_anyblok_dramatiq]: https://pypi.python.org/pypi/anyblok_dramatiq
[pypi_anyblok_dramatiq_svg]: https://img.shields.io/pypi/v/anyblok_dramatiq.svg
[dramatiq]: https://dramatiq.io
[AnyBlok_Multi_Engines]: https://github.com/AnyBlok/AnyBlok_Multi_Engines
[pypi_AnyBlok_Multi_Engines]: https://pypi.python.org/pypi/AnyBlok_Multi_Engines
[pypi_AnyBlok_Multi_Engines_svg]: https://img.shields.io/pypi/v/AnyBlok_Multi_Engines.svg
[anyblok_bus]: https://github.com/AnyBlok/anyblok_bus
[pypi_anyblok_bus]: https://pypi.python.org/pypi/anyblok_bus
[pypi_anyblok_bus_svg]: https://img.shields.io/pypi/v/anyblok_bus.svg
[anyblok_jsonschema]: https://github.com/AnyBlok/anyblok_jsonschema
[pypi_anyblok_jsonschema]: https://pypi.python.org/pypi/anyblok_jsonschema
[pypi_anyblok_jsonschema_svg]: https://img.shields.io/pypi/v/anyblok_jsonschema.svg
[AnyBlok]: https://github.com/AnyBlok/AnyBlok
[pypi_anyblok]: https://pypi.python.org/pypi/AnyBlok
[pypi_anyblok_svg]: https://img.shields.io/pypi/v/AnyBlok.svg
[postgresql]: https://www.postgresql.org/
[anyblok_sale]: https://github.com/AnyBlok/anyblok_sale
[pypi_anyblok_sale]: https://pypi.python.org/pypi/anyblok_sale
[pypi_anyblok_sale_svg]: https://img.shields.io/pypi/v/anyblok_sale.svg
[anyblok_address]: https://github.com/AnyBlok/anyblok_address
[pypi_anyblok_address]: https://pypi.python.org/pypi/anyblok_address
[pypi_anyblok_address_svg]: https://img.shields.io/pypi/v/anyblok_address.svg
