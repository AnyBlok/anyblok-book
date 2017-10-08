# Understand Anyblok

## Main features

You may wonder why AnyBlok is useful for you?

AnyBlok take advantages of famous python libraries and reuse packages
that already have proven their existences:

* Using [SQLAlchemy][sqlalchemy] AnyBlok is an [ORM][orm_wikipedia]:
  in few words you don't have to worry about the database management
  only your code source.
* With [Pyramid][pyramid_home] easily integrate any web protocol
  format, SOAP, JsonRPC, gRPC, REST.
* Integrated with [beaker][beaker] you'll be able to precisely manage
  web sessions and cache.

So you may use those librairies directly in your project but headaches
to make them working together by applying common patterns was already
spent by AnyBlok authors. Also it adds some nice feature by its own:

* Work with multiple database, to scale your application you could
  have one database for read/write and 3 others for read requests.
* Synchronize web session and SQL sessions
* User authorization are managed on actions (not models)
* Business API code: business code is disconnected from any graphical
  user interface so it can be use with any of them easily you may even
  integrate AnyBlok within existent python code.
* AnyBlok provide modularity mechanisms to let you easily reuse code
  distributed in different python packages.
* Make it easy to manage multiple levels of responsibilities in code
  source (for instance develop a generic product and manage
  customizations) by a dynamic inheritance mechanism.
* ...


## Anyblok Ecosystem

To illustrate components let's start with our vision about AnyBlok
ecosystem we would like to create and then we will deep in details with
AnyBlok components.

> **Note**: Keep in mind some of those features are forecast at the
> time I'm writing those lines.

```
+-----------------------------------------------------
| Furet UI
+----------------+------------------------------------
| Business Bloks UI
+----------------+------------------------------------
|                | Anyblok Furet UI
+ Business Bloks +------------------------------------
|                | Anyblok pyramid beaker
+----------+     +-----------------+--------------+-----------------------+-
| REA Blok |     | Anyblok Pyramid | Anyblok auth | Anyblok multi engines | 
+----------+-----+-----------------+--------------+-----------------------+-
| Anyblok
+-----------------------------------------------------
| SQL Alchemy
+-----------------------------------------------------
| SGBD: Postgresql / mariadb / SQL Lite / ...
+-----------------------------------------------------
```

As most ERP Product we would like to create an ERP ecosystem around
AnyBlok, at the time I'm writting those lines we have no business bloks
written. Also we believe a big effort must be done in tools rather than
start writting business code. We wants to give to developers
a strong and stable tools to help them to wirte business bloks.

We think business applications as a service so business rules
should be disconnected from the graphical user interface, we would
like to create some kind of business API that let developer free to use
common business bloks as he wants:

  - within its own existing base code
  - expose web services over http
  - let choice to connect to any kind of interfaces (Graphical or not)

[Furet UI][furetui] is web user interface
framework it can be use without AnyBlok as long the server implement
required interfaces (web services). The Idea of this UI framework is
that behaviors and representations are managed by server responses.
Developers do not require to write javascript to manage his
business application. Developer focus on writing backend code (the
business) by providing expected data. FureUI use famous web framework
likes [Vue.js][vuejs] to manage the the html page, [bulma][bulma] a
powerfull css framework.

[AnyBlok FuretUI][anyblok_furetui] is a
*Furet UI* backend that helps to generate graphical web interface
to AnyBlok it gives features likes:

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

[AnyBlok Pyramid beaker][anyblok_beaker] using the strength of
[beaker][beaker] you are able to precisely configure web sessions and
cache on top of pyramid.

[AnyBlok Pyramid][anyblok_pyramid] allow to easly expose web interfaces
with the power of the well known [pyramid framework][pyramid_home].
The main benefits is that it synchronized the web session with the
sql session to the developer do not have to worries about commits,
rollbacks, manage pool of sql connections and so on.

[SqlAlchemy][sqlalchemy] is a power full and well known orm that
let you writing python code and not worry about SQL. It also provide
the requiered abstractions so it can speak with different Database
system (Postgresql, mariadb, SQL Lite...)

Great but what is the value of AnyBlok their? I could ues Pyramid
and sqlAlchemy without AnyBlok if I only require a web service ?

Yes that true! here are some response of Anyblok values:

* It already do the work to make those greate python libraries works
  together.
* load python module at runtime: You can add blok at runtime, so you
  don't have to stop the service to add functionalities.
* With sames python packages installed in one environement, you can
  get multiple instance with different behaviors according which bloks
  you have chosen to install. This information is saved in the
  database so each database is an instance that can get a very different
  behaviours with only one running server, likes what is done in
  odoo an instance require a database to know which bloks are
  installed.
* You can easly separate code source in different bloks, in a blok
  you will be able to extend, overwrite or overload other bloks,
  this let you customize all components of an application without
  doing any change in blok develop by someone else. You have
  the full control to ordering module import and bloks dependencies.

[AnyBlok REA][anyblok_rea]: give abstract class that you can apply
to your business, [REA][rea] itself is a collection of business
patern which you can apply to manage stock, accounts and so on...

I'm writing those lines in the early age of the ecosystem,
when ecosystem will grow it will gains much value.
We are working on that way adding bloks to manage common used case
(ie: user managment with multiple authentications and authorization
mechanisme). Feel free to contribute, open issues, create pull request,
ask new repository through [github][gh_anyblok].

> **Important**: We belives splitting business code and graphical user
> interface is an important effort that have to be done as web
> framework involve faster than backend business code that why we
> would like to create business API.

## AnyBlok versus others

You may wonder why use Anyblok compared with other existing solutions.

### Odoo

AnyBlok mechanisms are fairly inspired from odoo. Here some
differences:

* Anyblok is 100% OpenSource
* AnyBlok do not create a new ORM it use an existing powerful one for
  years: [SqlAlchemy][sqlalchemy]
* With Anyblok you can use normal python packages to deliver and manage
  your code
* Backend and graphical user interface are split.
* You can manage web sessions as you want
*

### SqlAlchemy alone

### ERPNext



[orm_wikipedia]: https://en.wikipedia.org/wiki/Object-relational_mapping
[beaker]: https://github.com/bbangert/beaker
[rea]: https://en.wikipedia.org/wiki/Resources,_events,_agents_(accounting_model)
[sqlalchemy]: hhttp://www.sqlalchemy.org/
[pyramid_home]: https://trypyramid.com/
[gh_anyblok]: https://github.com/AnyBlok
[anyblok_furetui]: https://github.com/AnyBlok/anyblok_furetui
[anyblok_pyramid]: https://github.com/AnyBlok/anyblok_pyramid
[anyblok_beaker]: https://github.com/AnyBlok/AnyBlok_Pyramid_Beaker
[anyblok_rea]: https://github.com/AnyBlok/anyblok_rea
[anyblok_multi_engines]: https://github.com/AnyBlok/AnyBlok_Multi_Engines
[furetui]: https://github.com/AnyBlok/furet_ui
[vuejs]: https://vuejs.org/
[bulma]: http://bulma.io/
