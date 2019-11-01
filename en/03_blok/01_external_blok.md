## Add external Blok

We are going to use an existing generic business blok within our project to
represent postal addresses.

> **Note**: You can use ``II_setup-project`` directory from
> [AnyBlok/anyblok-book-examples][gh_abe] repository to get ready to start
> with this chapter.

### Add dependency

According to customers specifications we need to manage addresses, so
we are going to depend on the [address blok][gh_address_blok] developed
by the community and published in the [anyblok address python package
][pypi_address_blok].

To get the business blok, we need to install that package by adding it in
the package dependencies of the project:

```python
diff --git a/setup.py b/setup.py
index ca2ca86..1216cde 100644
--- a/setup.py
+++ b/setup.py
@@ -28,6 +28,7 @@ requirements = [
     'sqlalchemy,'
     'anyblok',
     'psycopg2-binary',
     'anyblok_pyramid',
+    'anyblok_address',
 ]

 test_requirements = [
```

To install new package dependencies use one of the following commands:

```bash
(rooms-venv)$ make setup-dev
# or
(rooms-venv)$ python setup.py develop
# or
(rooms-venv)$ pip install -e .
```

To install a new blok, you may add it as a requirement to a module
as shown in the [next chapter](./02_extend_blok.md) or install it
from command line:

```bash
(rooms-venv)$ anyblok_updatedb -c app.dev.cfg --install-bloks address
```

You can now add/remove addresses with the AnyBlok Python interpreter:
```python
anyblok_interpreter -c app.dev.cfg

In [11]: registry.Address.insert(
    first_name='Pierre',
    last_name='Verkest',
    street1='1 Rue de Stockholm',
    zip_code="75008",
    city="Paris",
    country='FRA'
)
Out[11]: <Address: 2ff62d0b-1168-4866-9cfa-f1ed4432d5b9, Pierre, Verkest, None, 75008, Country(alpha_2='FR', alpha_3='FRA', name='France', numeric='250', official_name='French Republic') [RO=False] >

registry.commit()
exit
```

[pypi_address_blok]: https://pypi.org/project/anyblok_address/
[gh_address_blok]: https://github.com/AnyBlok/anyblok_address/tree/master/anyblok_address/bloks/address
[gh_abe]: https://github.com/AnyBlok/anyblok-book-examples
