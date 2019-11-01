## AnyBlok versus others

You may wonder why using [AnyBlok][AnyBlok] over other existing
solutions. We don't really like to compare because we wouldn't probably
compete with others but in some conditions it can help to understand and make
a choice.

> **Disclaimer**: this part is how we understand other tools at the time we
> are writing this book. The world is moving, feel free to open a Pull Request
> and change the content of this book if we are wrong.

### Odoo

AnyBlok mechanisms are fairly inspired from odoo. Here are some
differences:

* AnyBlok is 100% OpenSource
* AnyBlok do not create a new ORM, it had used an existing powerful one for
  years: [SqlAlchemy][sqlalchemy]
* With AnyBlok you can use normal python packages to deliver and manage
  your code (which become possible in odoo with community efforts)
* Backend and graphical user interface are split
* With AnyBlok you can manage web sessions as you want
* AnyBlok reuses existing community packages as soon as it can

### SqlAlchemy / Pyramid

Below are some sorts of things that you don't have to worry with AnyBlok that you
should implement if you are using SQLAlchemy and Pyramid by yourself:

* AnyBlok makes it easy to extend models between different python packages
* AnyBlok allows to install different bloks provided by same packages to get
  different behaviors and models installed in the database
* Pyramid web session is synced with database transaction

But moving simple AnyBlok application code to pure SqlAlchemy wouldn't be
that hard.

[AnyBlok]: https://github.com/AnyBlok/AnyBlok
[pyramid_home]: https://trypyramid.com/
[sqlalchemy]: http://www.sqlalchemy.org/
