## Polymorphism

We have add some model to prepare this ``III-06-before_polymorphism`` branch
from [AnyBlok/anyblok-book-examples][gh_abe] repo you must clone this branch
before keep going:

```bash
git clone https://github.com/AnyBlok/anyblok-book-examples -b III-06-before_polymorphisme III-06-polymorphisme
cd III-06-polymorphisme
```

We have had some models to get start with polymorphism by:

* A **person** blok that contains following models
    * ``Person`` model that represent people with a name and firstname
    * ``Employee`` model that add position field on person model using
      polymorphism
* A **university** blok that represent all things regarding education
    * ``Professor`` model that extend Employee model using polymorphism


[Polymorphism][wiki_polymorphism] in AnyBlok allows to get clear business
readable queries as we will see later.

...

[gh_abe]: https://github.com/AnyBlok/anyblok-book-examples
[wiki_polymorphism]: https://en.wikipedia.org/wiki/Polymorphism_(computer_science)