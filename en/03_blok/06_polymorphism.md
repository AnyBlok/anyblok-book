## Polymorphism

You must clone the ``III-06-before_polymorphism`` branch
from [AnyBlok/anyblok-book-examples][gh_abe] repo before starting:

```bash
git clone https://github.com/AnyBlok/anyblok-book-examples -b III-06-before_polymorphisme III-06-polymorphisme
cd III-06-polymorphisme
```

We have added some models to get started with polymorphism:

* A **person** blok that contains following models
    * ``Person`` model that represents people with a name and firstname
    * ``Employee`` model that adds position field on person model using
      polymorphism
* A **university** blok that represents all things regarding education
    * ``Professor`` model that extends Employee model using polymorphism


[Polymorphism][wiki_polymorphism] in AnyBlok allows you to get clear business
readable queries as we will see later.

...

[gh_abe]: https://github.com/AnyBlok/anyblok-book-examples
[wiki_polymorphism]: https://en.wikipedia.org/wiki/Polymorphism_(computer_science)
