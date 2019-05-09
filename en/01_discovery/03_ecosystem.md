## AnyBlok Ecosystem

What we call `the Anyblok Ecosystem` is the collection of Python packages that
provide reusable Bloks or pluggable facilities to external Python libraries.

To illustrate this, let's start with our vision about the AnyBlok ecosystem *we
would like to exist*. Then we will cover the currently existing bloks in more depth.

As a quick reminder, don't forget that a reusable Blok is always inside a
Python package.
A Python package can expose several Bloks. A Blok provides models and behaviours
of its own or merely acts as a wrapper for an external library.

Anyblok's goal is to make developing business applications easier. To do that,
we think that a development team needs at least three different kinds of packages
to play with:

**Business bloks**: Functional and business oriented bloks
**Technical bloks**: Bloks that add technical features such as e.g., network 
protocols, data handling, session management... 
**Development tools**: Everything that makes development easier, faster and
reliable.

For now the easiest way to explore the Anyblok Ecosystem is Anyblok Organization
main github page [AnyBlokOrg]. Please note that all of those
repositories are tagged with topics according to the hereby categorization.

One of the main stakes with business applications is their trustworthiness.
To achieve that, we believe in those simple rules:

* Use existing robust and mature component is better than reinventing the wheel.
* The value of a reusable business Blok lies with its genericity and ability of
being usable for any use case. It's better to have a smaller scope that can be
useful for everyone than to address edge cases.
* Separate the business API from user interface is always a good idea because
it avoids polluting the business API with assumptions about one given user
interface.
* Unit testing, coverage, implementation examples are mandatory for a reusable
Blok.

> **Note**: Please, keep in mind that some of those features are forecasts at
> the time we're writing those lines, while some others are proof of concepts
> and some are stable to use in production.

### Business Bloks

[![Latest release on PyPI][pypi_anyblok_address_svg]][pypi_anyblok_address]
[AnyBlok address][anyblok_address] a blok for postal addresses normalization

[![Latest release on PyPI][pypi_anyblok_wms_base_svg]][pypi_anyblok_wms_base]
[AnyBlok WMS Base][anyblok_wms_base] these are base Bloks to build
Warehouse Management and Logistics applications.

[![Latest release on PyPI][pypi_anyblok_product_svg]][pypi_anyblok_product]
[AnyBlok Product][anyblok_product] provide product catalog management bloks.

[![Latest release on PyPI][pypi_anyblok_sale_svg]][pypi_anyblok_sale]
[AnyBlok sale][anyblok_sale] these are bloks used for sale managment.

[![Latest release on PyPI][pypi_anyblok_rea_svg]][pypi_anyblok_rea]
[AnyBlok REA][anyblok_rea] give abstract class that you can apply
to your business, [REA][rea] itself is a collection of business
patterns which you can apply to manage stocks, accounts and so on...

We're writing those lines in the early age of the ecosystem,
as it grow it will gain more value.
We are working in that way adding bloks to manage common use cases.
Feel free to contribute, open issues, create pull request,
ask new repository through [github][gh_anyblok].

### Technical Bloks

[![Latest release on PyPI][pypi_anyblok_beaker_svg]][pypi_anyblok_beaker]
[AnyBlok Pyramid beaker][anyblok_beaker] using the strength of
[beaker][beaker] you are able to precisely configure web sessions and cache
on top of [pyramid][pyramid_home].

[![Latest release on PyPI][pypi_anyblok-pyramid-rest-api-svg]
][pypi_anyblok-pyramid-rest-api]
[AnyBlok Pyramid rest api][AnyBlok-pyramid-rest-api] this blok provides
facilities for building restful APIs that interact with AnyBlok models
through a CRUD like pattern. It is based on [Cornice][cornice] that provides
helpers to build & document REST-ish Web Services with [Pyramid][pyramid_home],
with decent default behaviours. 

[![Latest release on PyPI][pypi_anyblok_bus_svg]][pypi_anyblok_bus]
[AnyBlok bus][anyblok_bus] lets AnyBlok send/receive messages on a BUS. Based
on [Pika](https://pika.readthedocs.io/en/stable/) Anybloks can communicate
through the well known [AMQP](https://www.amqp.org/). Also based on 
[marshmallow][marshmallow] models are easily serialized/de-serialized.

[![Latest release on PyPI][pypi_anyblok_jsonschema_svg]][pypi_anyblok_jsonschema]
[AnyBlok jsonschema][anyblok_jsonschema] this blok generates
[JSON Schema Draft v4](http://json-schema.org/) formatting from models using
[marshmallow-jsonschema](https://github.com/fuhrysteve/marshmallow-jsonschema)

[![Latest release on PyPI][pypi_anyblok_marshmallow_svg]][pypi_anyblok_marshmallow] 
[AnyBlok Marshmallow][anyblok_marshmallow] adds validator, serializer and
deserializer schema using [marshmallow][marshmallow]

[![Latest release on PyPI][pypi_anyblok_pyramid_svg]][pypi_anyblok_pyramid]
[AnyBlok Pyramid][anyblok_pyramid] allows to easily expose web interfaces
with the power of the well known [pyramid framework][pyramid_home].
The main benefit is that it synchronizes the web session with the
SQL session so that the developer does not have to worry about commits,
rollbacks, manage pool of SQL connections and so on.

[![Latest release on PyPI][pypi_anyblok_attachment_jinja_svg]
][pypi_anyblok_attachment_jinja]
[AnyBlok attachment jinja][anyblok_attachment_jinja] allows jinja template
in your reports.

[![Latest release on PyPI][pypi_anyblok_attachment_svg]][pypi_anyblok_attachment]
[AnyBlok attachment][anyblok_attachment] the base blok to manage
attachement and report systems.

[![Latest release on PyPI][pypi_anyblok_postgres_svg]][pypi_anyblok_postgres]
[AnyBlok postgres][anyblok_postgres] adds special features to AnyBlok that are
dedicated and based on [PostgreSQL][postgresql] features. Bloks (will) exploit
the strengh of:

* [JSONB field](
  https://www.postgresql.org/docs/current/static/datatype-json.html) which adds
  ``Jsonb`` column feature to AnyBlok
* [Materialized view](
  https://www.postgresql.org/docs/10/static/sql-creatematerializedview.html)
* ...

[![Latest release on PyPI][pypi_anyblok_io_svg]][pypi_anyblok_io]
[AnyBlok io][anyblok_io] provides bloks related to Input/Output (I/O) used
to import / export allowing to initialize data from files using any
format (xml, csv, ...).

[![Latest release on PyPI][pypi_anyblok_mixins_svg]][pypi_anyblok_mixins]
[AnyBlok mixins][anyblok_mixins] 
provides handy [mixins](https://en.wikipedia.org/wiki/Mixin) to speed up your
development, some features are:
* Make a model readonly
* Make a record readonly according to some conditions
* Add common columns (like id / create date / update date)
* Create workflow
* ...

[![Latest release on PyPI][pypi_anyblok_dramatiq_svg]][pypi_anyblok_dramatiq]
[AnyBlok dramatiq][anyblok_dramatiq]: [Dramatiq][dramatiq] is a distributed
task queueing library. This Blok makes it easy to use [Dramatiq][dramatiq]
with AnyBlok and distributes your workloads over multiple servers.

[![Latest release on PyPI][pypi_AnyBlok_Multi_Engines_svg]
][pypi_AnyBlok_Multi_Engines]
[AnyBlok Multi Engines][AnyBlok_Multi_Engines] this blok allows to use
multi database backends. Assuming you have one master with multiple
replicate databases, you can easily send read requests on replicates and
write on the master.

### Development tools

[![Latest release on PyPI][pypi_anyblok_svg]][pypi_anyblok]
[AnyBlok][AnyBlok] Scripts for creating or updating database and unit testing.

[![Latest release on PyPI][pypi_cookiecutter-anyblok-project_svg]]
[pypi_cookiecutter-anyblok-project] [Cookiecutter][cookiecutter] project
template for bootstraping Anyblok based projects.

[AnyBlokOrg]: https://github.com/AnyBlok
[AnyBlok]: https://github.com/AnyBlok/AnyBlok
[pypi_anyblok]: https://pypi.org/project/AnyBlok
[pypi_anyblok_svg]: https://img.shields.io/pypi/v/anyblok.svg
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
[cookiecutter]: https://github.com/audreyr/cookiecutter
