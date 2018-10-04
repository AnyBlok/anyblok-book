## AnyBlok Ecosystem


> **Important**: We believe splitting business code and graphical user
> interface is an important effort that has to be done as web
> framework evolve faster than backend business code that's why we
> would like to create business APIs.


To illustrate components let's start with our vision about the AnyBlok
ecosystem we would like to create and then we will go deep in details with
existing bloks.

> **Note**: Keep in mind some of those features are forecast at the
> time we're writing those lines, other are proof of concepts and some are
> stable to use in production.

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
around [AnyBlok][AnyBlok], at the time we're writing those lines there is
only few business bloks written. Also we believe a big effort must be done
in tools rather than start writing business code. We want to give to
developers strong and stable tools to help them write business bloks.

We think of business applications as a service so business rules
should be disconnected from the graphical user interface, we would
like to create some kind of business API that let developer free to use
common business bloks as he wants:

  - within its own existing base code
  - expose web services over http
  - let choose to connect to any kind of interfaces (Graphical or not)

### Business Bloks


[![Latest release on PyPI][pypi_anyblok_address_svg]][pypi_anyblok_address]
[AnyBlok address][anyblok_address] These are bloks for address management.

[![Latest release on PyPI][pypi_anyblok_wms_base_svg]][pypi_anyblok_wms_base]
[AnyBlok WMS Base][anyblok_wms_base] These are base Bloks to build
Warehouse Management and Logistics applications with AnyBlok.

[![Latest release on PyPI][pypi_anyblok_product_svg]][pypi_anyblok_product]
[AnyBlok Product][anyblok_product] provide product management bloks.

[![Latest release on PyPI][pypi_anyblok_sale_svg]][pypi_anyblok_sale]
[AnyBlok sale][anyblok_sale] These are bloks used for sale managment.

[![Latest release on PyPI][pypi_anyblok_rea_svg]][pypi_anyblok_rea]
[AnyBlok REA][anyblok_rea] give abstract class that you can apply
to your business, [REA][rea] itself is a collection of business
patern which you can apply to manage stock, accounts and so on...

We're writing those lines in the early age of the ecosystem,
as it grow it will gain more value.
We are working in that way adding bloks to manage common use cases.
Feel free to contribute, open issues, create pull request,
ask new repository through [github][gh_anyblok].


### Technical Bloks

[Furet UI][furetui] [POC not released] is a web user interface
framework that can be used without AnyBlok as long the server implement
required interfaces (web services). The Idea of this UI framework is
that behaviors and representations are managed by server responses.
Then developers do not require to write Javascript to manage their
business application. Developers focus on writing backend code (the
business) by providing expected data. FureUI use famous web frameworks
like [Vue.js][vuejs] to manage the HTML page, [bulma][bulma] a
powerful css framework.

[![Latest release on PyPI][pypi_anyblok_furetui_svg]][pypi_anyblok_furetui]
[AnyBlok FuretUI][anyblok_furetui] is a *Furet UI* backend that helps generating
 graphical web interface to AnyBlok. It gives features like:

* overload graphical interfaces between bloks:

  As with AnyBlok you are able to overload any python bloks,
  AnyBlok FuretUI let you overload the user interface:

  for instance, let's say you are using those bloks ``product`` and
  ``product_furet_ui`` which would be developed by someone
  else in the community that manage some basic product information (
  name, code, size, price), the other blok (``product_furet_ui``)
  manage how to display products in the graphical user interface.

  Imagine you want to extend the ``product_furet_ui`` functionality by
  adding EAN13 (barcode value). We would give you the following advice:
  create 2 bloks, the first one ``product_barcode`` will add fields and
  business methods to manage barcode on products. The second blok
  ``product_barecode_ui`` will extend the user interface defined in
  the ``product_furet_ui`` by adding the barcode field in the interface
  in a way to tell:
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
[marshmallow][marshmallow] models are easily serialized/de-serialized.

[![Latest release on PyPI][pypi_anyblok_jsonschema_svg]][pypi_anyblok_jsonschema]
[AnyBlok jsonschema][anyblok_jsonschema] This blok generate
[JSON Schema Draft v4](http://json-schema.org/) formatting from models using
[marshmallow-jsonschema](https://github.com/fuhrysteve/marshmallow-jsonschema)

[![Latest release on PyPI][pypi_anyblok_marshmallow_svg]][pypi_anyblok_marshmallow] 
[AnyBlok Marshmallow][anyblok_marshmallow] Add validator, serializer and
deserializer schema using [marshmallow][marshmallow]

[![Latest release on PyPI][pypi_anyblok_pyramid_svg]][pypi_anyblok_pyramid]
[AnyBlok Pyramid][anyblok_pyramid] allow to easily expose web interfaces
with the power of the well known [pyramid framework][pyramid_home].
The main benefit is that it synchronizes the web session with the
SQL session so that the developer do not have to worry about commits,
rollbacks, manage pool of SQL connections and so on.

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
  https://www.postgresql.org/docs/current/static/datatype-json.html) which add
  ``Jsonb`` column feature to AnyBlok
* [Materialized view](
  https://www.postgresql.org/docs/10/static/sql-creatematerializedview.html)
* ...

[![Latest release on PyPI][pypi_anyblok_io_svg]][pypi_anyblok_io]
[AnyBlok io][anyblok_io] Provide bloks related to Input/Output (I/O) used
to import / export allowing to initialize data from files using any
formats (xml, csv, ...).

[![Latest release on PyPI][pypi_anyblok_mixins_svg]][pypi_anyblok_mixins]
[AnyBlok mixins][anyblok_mixins] 
Provide handy [mixins](https://en.wikipedia.org/wiki/Mixin) to speed up your
development, some features are:
* Make a model readonly
* Make a record readonly according to some conditions
* Add common columns (like id / create date / update date)
* Create workflow
* ...

[![Latest release on PyPI][pypi_anyblok_dramatiq_svg]][pypi_anyblok_dramatiq]
[AnyBlok dramatiq][anyblok_dramatiq]: [Dramatiq][dramatiq] is a distributed
task queueing library. This Blok make it easy to use [Dramatiq][dramatiq]
with AnyBlok and distribute your workloads over multiple servers.

[![Latest release on PyPI][pypi_AnyBlok_Multi_Engines_svg]
][pypi_AnyBlok_Multi_Engines]
[AnyBlok Multi Engines][AnyBlok_Multi_Engines] This blok allow to use
multi database backends. Let's says you have one master with multiple
replicate databases, you can easily send read request on replicates and
writes to the master.


[AnyBlok]: https://github.com/AnyBlok/AnyBlok
[anyblok_address]: https://github.com/AnyBlok/anyblok_address
[pypi_anyblok_address]: https://pypi.python.org/pypi/anyblok_address
[pypi_anyblok_address_svg]: https://img.shields.io/pypi/v/anyblok_address.svg
[anyblok_attachment]: https://github.com/AnyBlok/anyblok_attachment
[pypi_anyblok_attachment]: https://pypi.python.org/pypi/anyblok_attachment
[pypi_anyblok_attachment_svg]: https://img.shields.io/pypi/v/anyblok_attachment.svg
[anyblok_attachment_jinja]: https://github.com/AnyBlok/anyblok_attachment_jinja
[pypi_anyblok_attachment_jinja]: https://pypi.python.org/pypi/anyblok_attachment_jinja
[pypi_anyblok_attachment_jinja_svg]: https://img.shields.io/pypi/v/anyblok_attachment_jinja.svg
[anyblok_beaker]: https://github.com/AnyBlok/AnyBlok_Pyramid_Beaker
[pypi_anyblok_beaker]: https://pypi.python.org/pypi/AnyBlok_Pyramid_Beaker
[pypi_anyblok_beaker_svg]: https://img.shields.io/pypi/v/anyblok_pyramid_beaker.svg
[anyblok_bus]: https://github.com/AnyBlok/anyblok_bus
[pypi_anyblok_bus]: https://pypi.python.org/pypi/anyblok_bus
[pypi_anyblok_bus_svg]: https://img.shields.io/pypi/v/anyblok_bus.svg
[anyblok_dramatiq]: https://github.com/AnyBlok/anyblok_dramatiq
[pypi_anyblok_dramatiq]: https://pypi.python.org/pypi/anyblok_dramatiq
[pypi_anyblok_dramatiq_svg]: https://img.shields.io/pypi/v/anyblok_dramatiq.svg
[anyblok_furetui]: https://github.com/AnyBlok/anyblok_furetui
[pypi_anyblok_furetui]: https://pypi.python.org/pypi/anyblok_furetui
[pypi_anyblok_furetui_svg]: https://img.shields.io/pypi/v/anyblok_furetui.svg
[anyblok_io]: https://github.com/AnyBlok/anyblok_io
[pypi_anyblok_io]: https://pypi.python.org/pypi/anyblok_io
[pypi_anyblok_io_svg]: https://img.shields.io/pypi/v/anyblok_io.svg
[anyblok_jsonschema]: https://github.com/AnyBlok/anyblok_jsonschema
[pypi_anyblok_jsonschema]: https://pypi.python.org/pypi/anyblok_jsonschema
[pypi_anyblok_jsonschema_svg]: https://img.shields.io/pypi/v/anyblok_jsonschema.svg
[anyblok_marshmallow]: https://github.com/AnyBlok/AnyBlok_Marshmallow
[pypi_anyblok_marshmallow]: https://pypi.python.org/pypi/AnyBlok_Marshmallow
[pypi_anyblok_marshmallow_svg]: https://img.shields.io/pypi/v/anyblok_marshmallow.svg
[anyblok_mixins]: https://github.com/AnyBlok/anyblok_mixins
[pypi_anyblok_mixins]: https://pypi.python.org/pypi/anyblok_mixins
[pypi_anyblok_mixins_svg]: https://img.shields.io/pypi/v/anyblok_mixins.svg
[AnyBlok_Multi_Engines]: https://github.com/AnyBlok/AnyBlok_Multi_Engines
[pypi_AnyBlok_Multi_Engines]: https://pypi.python.org/pypi/AnyBlok_Multi_Engines
[pypi_AnyBlok_Multi_Engines_svg]: https://img.shields.io/pypi/v/AnyBlok_Multi_Engines.svg
[anyblok_postgres]: https://github.com/AnyBlok/anyblok_postgres
[pypi_anyblok_postgres]: https://pypi.python.org/pypi/anyblok_postgres
[pypi_anyblok_postgres_svg]: https://img.shields.io/pypi/v/anyblok_postgres.svg
[anyblok_product]: https://github.com/AnyBlok/anyblok_product
[pypi_anyblok_product]: https://pypi.org/project/anyblok_product
[pypi_anyblok_product_svg]: https://img.shields.io/pypi/v/anyblok_product.svg
[anyblok_pyramid]: https://github.com/AnyBlok/anyblok_pyramid
[pypi_anyblok_pyramid]: https://pypi.python.org/pypi/anyblok_pyramid
[pypi_anyblok_pyramid_svg]: https://img.shields.io/pypi/v/Anyblok_Pyramid.svg
[AnyBlok-pyramid-rest-api]: https://github.com/AnyBlok/AnyBlok-pyramid-rest-api
[pypi_anyblok-pyramid-rest-api]: https://pypi.python.org/pypi/AnyBlok-pyramid-rest-api
[pypi_anyblok-pyramid-rest-api-svg]: https://img.shields.io/pypi/v/AnyBlok-pyramid-rest-api.svg
[anyblok_rea]: https://github.com/AnyBlok/anyblok_rea
[pypi_anyblok_rea]: https://pypi.python.org/pypi/anyblok_rea
[pypi_anyblok_rea_svg]: https://img.shields.io/pypi/v/anyblok_rea.svg
[anyblok_sale]: https://github.com/AnyBlok/anyblok_sale
[pypi_anyblok_sale]: https://pypi.python.org/pypi/anyblok_sale
[pypi_anyblok_sale_svg]: https://img.shields.io/pypi/v/anyblok_sale.svg
[anyblok_wms_base]: https://pypi.python.org/pypi/anyblok_wms_base
[pypi_anyblok_wms_base]: https://pypi.org/project/anyblok_wms_base
[pypi_anyblok_wms_base_svg]: https://img.shields.io/pypi/v/anyblok_wms_base.svg
[beaker]: https://github.com/bbangert/beaker
[dramatiq]: https://dramatiq.io
[bulma]: http://bulma.io/
[cornice]: https://cornice.readthedocs.io/en/latest/
[furetui]: https://github.com/AnyBlok/furet_ui
[gh_anyblok]: https://github.com/AnyBlok
[marshmallow]: https://marshmallow.readthedocs.io/en/latest/
[postgresql]: https://www.postgresql.org/
[pyramid_home]: https://trypyramid.com/
[rea]: https://en.wikipedia.org/wiki/Resources,_events,_agents_(accounting_model)
[sqlalchemy]: http://www.sqlalchemy.org/
[vuejs]: https://vuejs.org/
