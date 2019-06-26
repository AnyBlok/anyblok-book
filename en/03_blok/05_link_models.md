## Link models

We know that each room must be linked to an address. Lets make it true in our
project.

> **Note**: You can clone ``III-04_mixins`` branch from
> [AnyBlok/anyblok-book-examples][gh_abe] repo to get ready to start
> with this chapter.

### Add the field

In any database system, to link two tables together we use relationships, here
we need to add a ``Many2One`` relationship between ``Room`` and ``Address``:

```python
# File rooms_booking/room/room.py
@@ -8,6 +8,7 @@

 from anyblok import Declarations
 from anyblok.column import String, Integer
+from anyblok.relationship import Many2One

 Model = Declarations.Model
 Mixin = Declarations.Mixin
@@ -20,3 +21,6 @@ class Room(Mixin.IdColumn, Mixin.TrackModel):

     name = String(label="Room name", nullable=False, index=True)
     capacity = Integer(label="Capacity", nullable=False)
+    address = Many2One(
+        label="Address", model=Model.Address, nullable=False, one2many="rooms"
+    )
```

### Fix tests

You probably have noticed that your tests are failing now! We have to fix them.



```python
rooms_booking/room/tests/conftest.py
@@ -1 +1,15 @@
+import pytest
 from anyblok.conftest import *  # noqa: F401,F403
+
+
+@pytest.fixture()
+def an_address(rollback_registry):
+    return rollback_registry.Address.insert(
+        first_name="The Queen's College",
+        last_name="University of oxford",
+        street1="High Street",
+        zip_code="OX1 4AW",
+        city="Oxford",
+        country="GBR",
+        access="Kick the door to open it!"
+    )
```

```python
rooms_booking/room/tests/test_room.py
@@ -14,22 +13,24 @@ import pytz
 class TestRoom:
     """Test Room model"""

-    def test_create_room(self, rollback_registry):
+    def test_create_room(self, rollback_registry, an_address):
         registry = rollback_registry
         room_count = registry.Room.query().count()
         room = registry.Room.insert(
             name="A1",
             capacity=25,
+            address=an_address
         )
         assert registry.Room.query().count() == room_count + 1
         assert room.name == "A1"

-    def test_track_modification_date(self, rollback_registry):
+    def test_track_modification_date(self, rollback_registry, an_address):
         registry = rollback_registry
         before_create = datetime.now(tz=pytz.timezone(time.tzname[0]))
         room = registry.Room.insert(
             name="A1",
             capacity=25,
+            address=an_address
         )
         room.refresh()
         after_create = datetime.now(tz=pytz.timezone(time.tzname[0]))

```

If you are curious, you may read which kind of relationship AnyBlok provides
and learn from [documentation reference][doc_ref_relationship]

### Init some data

To play with queries we need to populate some data entries while installing
the blok.

```python
# File: rooms_booking/room/__init__.py

    def install(self):
        sorbonne = self.registry.Address.insert(
            first_name="La Sorbonne",
            last_name="La Chancellerie des Universités de Paris",
            street1="47, rue des Écoles ",
            zip_code="75230",
            city="Paris cedex 05",
            country="FRA",
            access="Crie fort pour réveiller le consièrge"
        )
        self.registry.Room.insert(
            name="Salle 101",
            capacity=25,
            address=sorbonne
        )
        self.registry.Room.insert(
            name="Salle 102",
            capacity=30,
            address=sorbonne
        )
        self.registry.Room.insert(
            name="Salle 103",
            capacity=28,
            address=sorbonne
        )
        trinity = self.registry.Address.insert(
            first_name="Trinity College",
            last_name="University of Oxford",
            street1="Broad Street",
            zip_code="OX1 3BH",
            city="Oxford",
            country="GBR",
            access="Ring the bell!"
        )
        self.registry.Room.insert(
            name="Room 101",
            capacity=47,
            address=trinity
        )
        self.registry.Room.insert(
            name="102",
            capacity=50,
            address=trinity
        )
        self.registry.Room.insert(
            name="103",
            capacity=42,
            address=trinity
        )
        imt_lille = self.registry.Address.insert(
            first_name="l'IMT Lille Douai",
            last_name="Université de Lille",
            street1="Site de Villeneuve d'Ascq",
            street2="20 rue Guglielmo Marconi",
            zip_code="59650",
            city="Villeneuve - d’Ascq",
            country="FRA",
            access="Ring the bell!"
        )
        self.registry.Room.insert(
            name="Salle E001S",
            capacity=42,
            address=imt_lille
        )
        self.registry.Room.insert(
            name="Salle E002S",
            capacity=28,
            address=imt_lille
        )
        self.registry.Room.insert(
            name="Salle E003S",
            capacity=60,
            address=imt_lille
        )
        self.registry.Room.insert(
            name="Amphi Byron",
            capacity=200,
            address=imt_lille
        )
        self.registry.Room.insert(
            name="Amphi Pascal",
            capacity=150,
            address=imt_lille
        )
        self.registry.Room.insert(
            name="Amphi Morse",
            capacity=500,
            address=imt_lille
        )
        self.registry.Room.insert(
            name="Amphi Shannon",
            capacity=200,
            address=imt_lille
        )
        self.registry.Room.insert(
            name="Amphi Chappe",
            capacity=500,
            address=imt_lille
        )

```

As this code is only called while installing this blok we are going to recreate
the dev database:

``bash
dropdb rooms_booking
make setup-dev
``

### Play with queries

As AnyBlok is based on **SqlAlchemy** you can directly learn how to build
queries from [SqlAlchemy query API doc][sqlalchemy_query]. Here are some examples
using ``anyblok_interpreter -c app.dev.cfg``

* How many addresses we have

```python
registry.Address.query().count()
3
```

* Count how many rooms have a capacity greater than 45

```python
registry.Room.query().filter(
    registry.Room.capacity > 45).count()
8
```

* Count how many rooms have a capacity greater than 45 in France

```python
registry.Room.query().join(
    registry.Room.address).filter_by(
    country='FRA').filter(
    registry.Room.capacity > 45).count()
6
```

* Display the query that will be execute

```python
str(
    registry.Room.query().join(
        registry.Room.address).filter_by(
        country='FRA').filter(
        registry.Room.capacity > 45)
)
'SELECT room.capacity AS room_capacity, room.name AS room_name, room.id AS room_id, room.create_date AS room_create_date, room.edit_date AS room_edit_date, room.address_uuid AS room_address_uuid
 FROM room JOIN address ON address.uuid = room.address_uuid
 WHERE address.country = %(country_1)s AND room.capacity > %(capacity_1)s'
```

* Count how many rooms have a capacity greater than 45 **OR** in France

```python
from sqlalchemy import or_

registry.Room.query().join(
    registry.Room.address).filter(
    or_(
        registry.Address.country=='FRA',
        registry.Room.capacity > 45
    )
).count()
13
```

* Display a list of tuple (room name, address first name, room capacity) using the previous query.

```Python
registry.Room.query(
    registry.Room.name,
    registry.Address.first_name,
    registry.Room.capacity
).join(
    registry.Room.address
).filter(
    or_(
        registry.Address.country=='FRA',
        registry.Room.capacity > 45)
).all()
[('Salle 101', 'La Sorbonne', 25),
 ('Salle 102', 'La Sorbonne', 30),
 ('Salle 103', 'La Sorbonne', 28),
 ('Room 101', 'Trinity College', 47),
 ('102', 'Trinity College', 50),
 ('Salle E001S', "l'IMT Lille Douai", 42),
 ('Salle E002S', "l'IMT Lille Douai", 28),
 ('Salle E003S', "l'IMT Lille Douai", 60),
 ('Amphi Byron', "l'IMT Lille Douai", 200),
 ('Amphi Pascal', "l'IMT Lille Douai", 150),
 ('Amphi Morse', "l'IMT Lille Douai", 500),
 ('Amphi Shannon', "l'IMT Lille Douai", 200),
 ('Amphi Chappe', "l'IMT Lille Douai", 500)]
```

* ... and order results by capacity

```python
registry.Room.query(
    registry.Room.name,
    registry.Address.first_name,
    registry.Room.capacity
).join(registry.Room.address).filter(
    or_(
        registry.Address.country == 'FRA',
        registry.Room.capacity > 45)
).order_by(
    registry.Room.capacity
).all()
[('Salle 101', 'La Sorbonne', 25),
 ('Salle 103', 'La Sorbonne', 28),
 ('Salle E002S', "l'IMT Lille Douai", 28),
 ('Salle 102', 'La Sorbonne', 30),
 ('Salle E001S', "l'IMT Lille Douai", 42),
 ('Room 101', 'Trinity College', 47),
 ('102', 'Trinity College', 50),
 ('Salle E003S', "l'IMT Lille Douai", 60),
 ('Amphi Pascal', "l'IMT Lille Douai", 150),
 ('Amphi Byron', "l'IMT Lille Douai", 200),
 ('Amphi Shannon', "l'IMT Lille Douai", 200),
 ('Amphi Chappe', "l'IMT Lille Douai", 500),
 ('Amphi Morse', "l'IMT Lille Douai", 500)]
```

* Get only the first element

```Python
registry.Room.query(
    registry.Room.name, registry.Address.first_name, registry.Room.capacity
).join(
    registry.Room.address
).filter(
    or_(registry.Address.country=='FRA', registry.Room.capacity > 45)
).order_by(registry.Room.capacity).first()
('Salle 101', 'La Sorbonne', 25)
```

> **Note**: You can clone ``III-05_link-models`` branch from
> [AnyBlok/anyblok-book-examples][gh_abe] repo to get that whole code.


[sqlalchemy_query]: https://docs.sqlalchemy.org/en/latest/orm/query.html
[gh_abe]: https://github.com/AnyBlok/anyblok-book-examples
[doc_ref_relationship]: http://doc.anyblok.org/en/latest/MEMENTO.html#relationship
