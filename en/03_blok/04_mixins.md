## Using mixins

Now we are going to use AnyBlok [mixins][wikipedia_mixin] to:
* Refactor our code in order to use ``IdColumn`` mixin to create the id
  primary key column on the room model
* Use the ``TrackModel`` mixin that adds 2 fields to track creation and last
  modification date:

> **Note**: You can clone ``III-03_create-model`` branch from
> [AnyBlok/anyblok-book-examples][gh_abe] repo to get ready to start
> with this chapter.


### Install ``anyblok_mixins``

Add [anyblok_mixins][pypi_anyblok_mixins] to your package requirements and
install the package.

```python
 # file: setup.py

 requirements = [
     'anyblok',
     'psycopg2',
     'anyblok_pyramid',
     'anyblok_address',
+    'anyblok_mixins',
 ]

```

Declare ``anyblok-mixins`` blok as required in the Room blok init file.

```python
  # file: rooms_booking/room/__init__.py

  class Room(Blok):
     """Room's Blok class definition
     """
     version = "0.1.0"
     author = "Pierre Verkest"
-    required = ['anyblok-core', 'address']
+    required = ['anyblok-core', 'address', 'anyblok-mixins']

```

### Add test

In the following test we make sure as requested by our customer that
``edit_date`` field is properly updated after a change occurs on a room.

```python
# file: rooms_booking/room/tests/test_room.py

from datetime import datetime
import time
import pytz


class TestRoom:
    """Test Room model"""

    [...]

    def test_track_modification_date(self, rollback_registry):
        registry = rollback_registry
        before_create = datetime.now(tz=pytz.timezone(time.tzname[0]))
        room = registry.Room.insert(
            name="A1",
            capacity=25,
        )
        room.refresh()
        after_create = datetime.now(tz=pytz.timezone(time.tzname[0]))
        room.name = "A2"
        registry.flush()
        after_edit = datetime.now(tz=pytz.timezone(time.tzname[0]))
        assert before_create <= room.create_date <= after_create
        assert after_create <= room.edit_date <= after_edit
```

> *Note*: ``room.flush()`` is used here to force sending data to the database
> as ``edit_date`` field is configured with [auto_update][ref_doc_auto_update]
> option wich is called before sending data to the database.

> *Note*: ``room.refresh()`` is used to clear cache on the ORM side to
> request data from the database. It will perform a flush before if
> necessary.

Those methods are rarely used in your code but can be tricky while playing with
some kind of fields.

### Reuse mixins

Using multiple inheritance capability we inherits from
``Declarations.Mixin.IdColumn`` and ``Declarations.Mixin.TrackModel``.
Diff result:

```python
 from anyblok import Declarations
 from anyblok.column import String, Integer

 Model = Declarations.Model
+Mixin = Declarations.Mixin
+
 register = Declarations.register


 @register(Model)
-class Room:
+class Room(Mixin.IdColumn, Mixin.TrackModel):

-    id = Integer(primary_key=True)
     name = String(label="Room name", nullable=False, index=True)
     capacity = Integer(label="Capacity", nullable=False)
```

You should now be able to launch previousily added tests and make them pass.


[gh_abe]: https://github.com/AnyBlok/anyblok-book-examples
[wikipedia_mixin]: https://en.wikipedia.org/wiki/Mixin
[ref_doc_auto_update]: http://doc.anyblok.org/en/latest/MEMENTO.html#column<
[pypi_address_blok]: https://pypi.org/project/anyblok_mixins/
