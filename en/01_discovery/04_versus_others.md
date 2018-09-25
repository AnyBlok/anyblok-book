## AnyBlok versus others

You may wonder why use [AnyBlok][AnyBlok] compared with other existing
solutions. We don't really likes to compare here because we would'nt probably
fare with others but in some conditions it can helps to understand and make
a choice.

> **Disclamer**: this part is how we understand other tools at the time we
> are writting this book. World is moving feel free to open a Pull Request
> and change the content of this book if we are wrong.

### Odoo

AnyBlok mechanisms are fairly inspired from odoo. Here some
differences:

* AnyBlok is 100% OpenSource
* AnyBlok do not create a new ORM it use an existing powerful one for
  years: [SqlAlchemy][sqlalchemy]
* With AnyBlok you can use normal python packages to deliver and manage
  your code (Wich becomes possible in odoo with community efforts)
* Backend and graphical user interface are split.
* With AnyBlok you can manage web sessions as you want
* AnyBlok reuse existing community packages as soon as it can

### SqlAlchemy / Pyramid

Below some sorts of things that you don't have to worry with AnyBlok that you
should implement if you were using SQLAlchemy and Pyramid by yourself:

* AnyBlok make easy to extend models betweens differents python packages
* AnyBlok allow to install different bloks provide by same packages to get
  differnts behaviours and model installed in the database
* Pyramid web session is synced with database transaction

But moving simple AnyBlok application code to pure SqlAlchemy wouldn't be
that hard.

[AnyBlok]: https://github.com/AnyBlok/AnyBlok
[pyramid_home]: https://trypyramid.com/
[sqlalchemy]: http://www.sqlalchemy.org/
