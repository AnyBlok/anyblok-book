# III - First Bloks

In the previous section you learned how to create an AnyBlok project tree
ready to hack using [anyblok cookiecutter project template](
https://github.com/AnyBlok/cookiecutter-anyblok-project).

You can clone ``II_setup-project`` branch from [AnyBlok/anyblok-book-examples](
https://github.com/AnyBlok/anyblok-book-examples) repo to get ready to start
this section:

```bash
# clone 
$ git clone -b II_setup-project https://github.com/AnyBlok/anyblok-book-examples anyblok-book-examples-III
$ cd anyblok-book-examples-III
$ source ../rooms-venv/bin/activate
(rooms-venv) $
```

In this section you will learn how to add external bloks, extend existing model
provided by other bloks, create new model, link models together.

For our project we know that:

* Each rooms are located in one address. 
* An university may have multiple addresses and each building will be
  represent by a different address.
* On each address we needs the capabilities to collect access information
  to get into the building as they sometimes use code for restricted
  laboratories access or requiere a key to open the main door...
* A room is specified by:    
    * name: the room name or room number
    * address: The address linked to the room
    * capacity: how many people can be sit behind a desk

In this section we will create a python API using multiple bloks with following
requirements:

* We should automatically track last change date when room information change
  but we don't need to keep the history of those changes
* We should be able to count the number of rooms
    * per address
    * per university

* [Add external Blok](01_external_blok.md)
* [Extend Blok](02_extend_blok.md)
* [Create new model](03_create_model.md)
* [Use mixins](04_mixins.md)
* [link models](05_link_models.md)
* [Exercise](06_exercices.md)
