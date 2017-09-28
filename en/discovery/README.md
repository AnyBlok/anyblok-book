# Understand Anyblok

You may wonder why AnyBlok is useful for you. We will try to give
most features provide by AnyBlok and help you to compare it with
other solutions.

To helps contributors we start with our vision about the ecosystem
we would like to create and then go in thuther details with AnyBlok
components.

> **Note**: Keep in mind some of those features are forecast at the
> time I'm writing those lines.

```
+-----------------------------------------------------
| Furet UI
+----------------+------------------------------------
| Business Bloks UI
+----------------+------------------------------------
| Business Bloks | Anyblok Furet UI
+----------+     +-----------------+--------------+---
| REA Blok |     | Anyblok Pyramid | Anyblok auth |
+----------+-----+-----------------+--------------+---
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
a strong and stable tools to help him to wirte business bloks.

We think business applications as a service so business rules
should be disconnected from the user graphical interface, we would
like to create some kind of business API that let developer free to use
common business bloks as he wants likes:
  - integrate it to its own existing base code
  - expose web services over http
  - let choice to connect to any kind of interfaces (Graphical or not)

[Furet UI][furetui] is web user interface
framwork it can be use without Anyblok as long the server implement
required interfaces (web serivces). The Idea of this framwork is that
the interface behavior and representation is manage by server response
so, the developer do not require to write javascript to manage his
business application. Developer can focus on writing backend code by
providing expected data.

[AnyBlok FuretUI][anyblok_furetui] is a
*Furet UI* backend that helps to generate graphical web interface
to AnyBlok it expecte to give some features likes:

* oververload graphical interfaces between bloks: for instance
  let say we have a blok called ``product`` that manage some basic
  information regarding them (name, code, size, price), an other
  blok called ``product_furet_ui`` that manage how to display product
  in the user interface. Imagine you want to extend the
  ``product_furet_ui`` functionality by adding EAN13 (barcode value).
  We would give you the following advice to create 2 bloks that
  to get the following dependencies:

  ```
  ├── product_barcode
  │   └── product
  │       └── anyblok
  └── product_barcode_ui
      └── product_furet_ui
          └── anyblok_furet_ui
  ```

  So the ``product_barcode`` will add fields and business
  methods to manage barecode on products. ``product_barecode_ui``
  will extend the user interface defined in the ``product_furet_ui``
  by adding the barcode value in the interface in the way that tell say:
  "hey! I would like to add the field 'barcode' under the existing field
  product code"

[AnyBlok Pyramid][anyblok_pyramid] allow to easly expose web interface
with the power of the well known [pyramid framework][pyramid_home].
The main benefits is that it syncrhonized the web session with the
sql session to the developer do not have to worries about commit,
rollback, manage poll of sql connections and so on.

[SqlAlchemy][sqlalchemy] is a power full and well known orm that
let you writing python code and not worry about SQL. It also provide
the requiered abstractions so it can speak with differents Database
system (Postgresql, mariadb, SQL Lite...)

Great but what is the value of AnyBlok their? I could ues Pyramid
and sqlAlchemy without AnyBlok if I only require a web service ?

Yes that true! here are some response of Anyblok values:

* load python module at runtime: You can add blok at runtime, so you
  don't have to stop the service to add functionalities.
* With sames python packages installed in one environement, you can
  get multiple instance with different behaviors according which bloks
  you have chosen to install. This information is saved in the
  database so each database is an instance that can get a very different
  behaviours with only one running server, likes what is done in
  odoo an instance require a database to know wich bloks are
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


[rea]: https://en.wikipedia.org/wiki/Resources,_events,_agents_(accounting_model)
[anyblok_rea]: https://github.com/AnyBlok/anyblok_rea
[sqlalchemy]: hhttp://www.sqlalchemy.org/
[furetui]: https://github.com/AnyBlok/furet_ui
[anyblok_furetui]: https://github.com/AnyBlok/anyblok_furetui
[anyblok_pyramid]: https://github.com/AnyBlok/anyblok_pyramid
[gh_anyblok]: https://github.com/AnyBlok
[pyramid_home]: https://trypyramid.com/
