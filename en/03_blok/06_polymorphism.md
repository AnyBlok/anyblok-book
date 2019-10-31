## Polymorphism

You must clone the ``III-06-before_polymorphism`` branch
from [AnyBlok/anyblok-book-examples][gh_abe] repository before starting:

```bash
git clone https://github.com/AnyBlok/anyblok-book-examples -b III-06-before_polymorphisme III-06-polymorphisme
cd III-06-polymorphisme
```

[Polymorphism][wiki_polymorphism] in AnyBlok allows you to share same database
table with different models.

It helps building elegant and comprehensive Python APi, each polymorphic model
can be accessed through a different namespace (Employee vs
Employee.Professor for example).
Namespacing is also helpful for giving different behaviours per polymorphic
models

We have added some models to get started with polymorphism:

* A **person** blok that contains following models
    * ``Person`` model that represents people with a name and firstname
    * ``Employee`` model that adds position field on person model using
      polymorphism
* A **university** blok that represents all things regarding education
    * ``Professor`` model that extends *Employee* model using polymorphism
    * ``Student`` model that extends *Person* model using polymorphism

As you can see a ``Professor`` is an ``Employee`` of another kind. Anyblok
polymorphism will help for:

### Querying all persons through registry.Person namespace

```python
In [1]: registry.Person.query().all()
Out[1]: [<Model.Person(first_name='Alice', id=1, last_name='Wunderer', person_type='person')>,
 <Model.Person.Employee(first_name='John', id=2, last_name='Doe', person_type='employee', position=<not loaded>)>]
```

### Querying all employees through registry.Person.Employee

```python
In [2]: registry.Person.Employee.query().all()
Out[2]: [<Model.Person.Employee(first_name='John', id=2, last_name='Doe', person_type='employee', position='Accountant')>] 


We also add the ``University`` model thats links to the ``Address`` model, an
university could have from zero to N addresses.


...

[gh_abe]: https://github.com/AnyBlok/anyblok-book-examples
[wiki_polymorphism]: https://en.wikipedia.org/wiki/Polymorphism_(computer_science)
