# III - First Bloks

In the previous section, you learned how to create an AnyBlok project tree
ready to hack using [anyblok cookiecutter project template](
https://github.com/AnyBlok/cookiecutter-anyblok-project).

You can clone ``II_setup-project`` branch from [AnyBlok/anyblok-book-examples](
https://github.com/AnyBlok/anyblok-book-examples) repo to get ready to start
this section:

```bash
# clone 
git clone -b II_setup-project https://github.com/AnyBlok/anyblok-book-examples anyblok
cd anyblok-book-examples
source ../venv
```
In this section, you will learn how to add: 

* External bloks
* Extend existing models provided by other bloks
* Create a new model or link models together

For our project we assume that:

* Each rooms are located with one address.
* An university may have multiple addresses and each building will be
  represented with a different address.
* For each address, we need to be able to store access information
  to get into the building as they sometime use code for restricted
  laboratories, require a key to open the main door, etc...
* A room is specified by:
    * name: the room name or room number
    * address: the address linked to the room
    * capacity: how many people can be sit behind a desk

In this section, we will create a Python API using multiple bloks with the
following requirements:

* We should track the last change date on a room
* We should be able to count the number of rooms
    * per address
    * per university


* [Add external Blok](01_external_blok.md)
* [Extend Blok](02_extend_blok.md)
* [Create new model](03_create_model.md)
* [Use mixins](04_mixins.md)
* [link models](05_link_models.md)
* [link models](06_polymorphism.md)
* [Exercise](07_exercices.md)
