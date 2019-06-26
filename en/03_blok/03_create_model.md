## Create a new model

In this chapter we will see how to create a new ``Room`` model.

> **Note**: You can clone ``III-02_extend-blok`` branch from
> [AnyBlok/anyblok-book-examples][gh_abe] repo to get ready to start
> with this chapter.


As explained in the [section intro](README.md) a room is described as:

* name: The room name or room number
* address: The address linked to the room
* capacity: The quantity of people who can sit behind a desk

Let's create a simple test that checks if we can insert a new room with name
and capacity.
Then we will see how to link models (Room and Address) in [chapter 5 of this
section](./05_link_models.md)

```python
# file: rooms_booking/room/tests/test_room.py


class TestRoom:
    """Test Room model"""

    def test_create_room(self, rollback_registry):
        registry = rollback_registry
        room_count = registry.Room.query().count()
        room = registry.Room.insert(
            name="A1",
            capacity=25,
        )
        assert registry.Room.query().count() == room_count + 1
        assert room.name == "A1"

```

Then we implement the new model creating a new python module.

```python
# file: rooms_booking/room/room.py

from anyblok import Declarations
from anyblok.column import String, Integer

Model = Declarations.Model
register = Declarations.register


@register(Model)
class Room:

    id = Integer(primary_key=True)
    name = String(label="Room name", nullable=False, index=True)
    capacity = Integer(label="Capacity", nullable=False)

```

> **Note**: AnyBlok requires a primary key because the SQLAlchemy ORM needs it
> as exmplain in [this SQLAlchemy FAQ](
> https://docs.sqlalchemy.org/en/rel_1_1/faq/ormconfiguration.html#faq-mapper-primary-key)

Mind to import your new module ``room.py`` in the *Blok* definition:

```python
# file: ``/rooms_booking/room/__init__.py``
# class: Room(Blok)

     @classmethod
     def import_declaration_module(cls):
         """Python module to import in the given order at start-up
         """
         from . import address  # noqa
+        from . import room # noqa

     @classmethod
     def reload_declaration_module(cls, reload):
         """Python module to import while reloading server (ie when
         adding Blok at runtime
         """
         from . import address  # noqa
+        from . import room  # noqa
         reload(address)
+        reload(room)

```

Your test should pass now:

```bash
# upgrade the model to create new table in the database
anyblok_updatedb -c app.test.cfg --update-bloks room
[...]
# run your tests
(rooms-venv) $ make test
ANYBLOK_CONFIG_FILE=app.test.cfg py.test -v -s rooms_booking
========================================== test session starts ==========================================
platform linux -- Python 3.5.3, pytest-4.6.3, py-1.8.0, pluggy-0.12.0 -- ~/anyblok/venvs/book/bin/python3
cachedir: .pytest_cache
rootdir: ~/anyblok/anyblok-book-examples, inifile: tox.ini
plugins: cov-2.7.1
collected 2 items                                                                                       

rooms_booking/room/tests/test_address.py::TestAddress::test_create_address AnyBlok Load init: EntryPoint.parse('anyblok_pyramid_config = anyblok_pyramid:anyblok_init_config')
Loading config file '/etc/xdg/AnyBlok/conf.cfg'
Loading config file '~/.config/AnyBlok/conf.cfg'
Loading config file '~/anyblok/anyblok-book-examples/app.test.cfg'
Loading config file '~/anyblok/anyblok-book-examples/app.cfg'
Loading config file '/etc/xdg/AnyBlok/conf.cfg'
Loading config file '~/.config/AnyBlok/conf.cfg'
PASSED
rooms_booking/room/tests/test_room.py::TestRoom::test_create_room PASSED
[...]
================================= 2 passed, 1 warnings in 2.13 seconds ==================================
```


[gh_abe]: https://github.com/AnyBlok/anyblok-book-examples
