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
from anyblok.tests.testcase import BlokTestCase


class TestRoom(BlokTestCase):
    """ Test python api on AnyBlok models"""

    def test_create_room(self):
        room_count = self.registry.Room.query().count()
        room = self.registry.Room.insert(
            name="A1",
            capacity=25,
        )
        self.assertEqual(
            self.registry.Room.query().count(),
            room_count + 1
        )
        self.assertEqual(
            room.name,
            "A1"
        )
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
anyblok_nose -c app.test.cfg -- -v -s rooms_booking
AnyBlok Load init: EntryPoint.parse('anyblok_pyramid_config = anyblok_pyramid:anyblok_init_config')
Loading config file '/etc/xdg/AnyBlok/conf.cfg'
Loading config file '/home/pverkest/.config/AnyBlok/conf.cfg'
Loading config file '/home/pverkest/anyblok/anyblok-book-examples/app.test.cfg'
Loading config file '/home/pverkest/anyblok/anyblok-book-examples/app.cfg'
AnyBlok Load init: EntryPoint.parse('anyblok_pyramid_config = anyblok_pyramid:anyblok_init_config')
Loading config file '/etc/xdg/AnyBlok/conf.cfg'
Loading config file '/home/pverkest/.config/AnyBlok/conf.cfg'
test_create_address (rooms_booking.room.tests.test_address.TestAddress) ... ok
test_create_room (rooms_booking.room.tests.test_room.TestRoom) ... ok

----------------------------------------------------------------------
Ran 2 tests in 0.868s

OK
```


[gh_abe]: https://github.com/AnyBlok/anyblok-book-examples
