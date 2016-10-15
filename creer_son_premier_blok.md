# Créer son premier Blok

Les blocs sont definis dans des pacquets python standard, un pacquet pouvant contenir plusieurs blok

## Créer un paquet python                                                       
                                                                                
lire le tutoriel sur les paquets python pour le pypi                               
http://sametmax.com/creer-un-setup-py-et-mettre-sa-bibliotheque-python-en-ligne-sur-pypi/
                                                                                
Structure du package pour l'exemple
                                                                                
> anyblok_todolist                                                                
> .. anyblok_todolist                                                             
> .. .. __init__.py                                                               
> .. .. todolist                                                                  
> .. .. .. __init__.py                                                            
> .. setup.py                                                                     
                                                                                
### Fichier setup

Le fichier setup doit définir l'entry_point bloks. Celui ci pointe vers une module
python, Ce module porte la déclaration du blok.

> **WARNING** Le module doit être un repertoire avec un fichier __init__.py. Tous les fichiers et répertoires a l'interrieur seront définit comme appartenant au blok.

```python                                                                              
from setuptools import setup, find_packages                                 
                                                                            
requires = [                                                                
    'anyblok',                                                              
]                                                                           
                                                                            
setup(                                                                      
    name="anyblok_todolist",                                                
    version='0.1.0',                                                        
    author="Jean-Sébastien Suzanne",                                        
    author_email="jssuzanne@anybox.fr",                                     
    description="Todolist for AnyBlok",                                     
    license="MPL2",                                                         
    long_description="Tiny todolist for AnyBlok",                           
    packages=find_packages(),                                               
    zip_safe=False,                                                         
    include_package_data=True,                                              
    install_requires=requires,                                                 
    tests_require=requires + ['nose'],                                      
    classifiers=[                                                           
        'Intended Audience :: Developers',                                  
        'Programming Language :: Python :: 3.3',                            
        'Programming Language :: Python :: 3.4',                            
        'Programming Language :: Python :: 3.5',                            
        'Topic :: Software Development :: Libraries :: Python Modules',     
    ],                                                                      
    entry_points={                                                          
        'bloks': [                                                          
            'todolist=anyblok_todolist.todolist:BlokTodoList',              
        ],                                                                  
    },                                                                      
    extras_require={},                                                      
)                        
```
                                                                                   
### Fichier de description du blok 
                                
Il est definit dans le repertoire ciblé par l'entry_point:
> anyblok_todolist/anyblok_todolist/todolist/__init__.py

La declaration du blok est faite par la création d'une class qui hérite de Blok. La seule information obligatoire est le numero de version.

```python                                                                              
from anyblok.blok import Blok                                               
                                                                            
                                                                            
class BlokTodoList(Blok):                                                   
    version = '0.1.0'                                                       
```
                                                                                
                                                                                
## Installer le nouveau package.

Le nouveau paquet n'étant pas sur le **pypi**, nous allons l'installé a l'ancienne.

> cd anyblok_todolist
> ../sandbox/bin/python setup.py develop
> cd ..
                                                                                
## Vérifier que le nouveau blok existe.

Il suffit de ce connecter a la base AnyBlok, La déclaration du blok étant fait dans le setup via l'entry_point blok, Il est donc reconnu au démarage du console script.

> sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
> ==> In [1]: registry.System.Blok.query().all()                                  
> ==> Out[1]:                                                                     
> ==> [anyblok-core (installed),                                                  
> ==>  anyblok-io (uninstalled),                                                  
> ==>  anyblok-io-csv (uninstalled),                                              
> ==>  anyblok-io-xml (uninstalled),                                              
> ==>  model_authz (uninstalled),                                                 
> ==>  todolist (uninstalled)]                                                    
                                                                                
## Installer le blok todolist

Chaque blok peut être installé / désinstallé / ou mis à jour. Le schema de la base de donnée est mise a jour en consequence.

Il exist deux possibilités pour installer un blok:
* Via l'interpreteur
* Via le console script **anyblok_updatedb**

### Via l'interpreteur

Le registry concentre la totalité des objet bas niveau d'anyblok comme la session de SQLAlchemy, il possède également la methode **upgrade**

> sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
> ==> In [1]: registry.upgrade(install=('todolist',))
> ==> In [2]: registry.System.Blok.query().all()                                  
> ==> Out[2]:                                                                     
> ==> [anyblok-core (installed),                                                  
> ==>  anyblok-io (uninstalled),                                                  
> ==>  anyblok-io-csv (uninstalled),                                              
> ==>  anyblok-io-xml (uninstalled),                                              
> ==>  model_authz (uninstalled),                                                 
> ==>  todolist (installed)]       
> ==> In [3]: registry.commit()

> **WARNING**: Si vous ne commitez pas, alors le schema ne sera pas enregistré dans la base de donnée et a la sortie de l'interpreteur, le blok ne sera plus installé.

### Via le console script

Le résultat est le même car le console script appel la methode **upgrade**.

> sandbox/bin/anyblok_updatedb --db-name todolist --db-driver-name postgresql --install-bloks todolist
> sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
> ==> In [1]: registry.System.Blok.query().all()                                  
> ==> Out[1]:                                                                     
> ==> [anyblok-core (installed),                                                  
> ==>  anyblok-io (uninstalled),                                                  
> ==>  anyblok-io-csv (uninstalled),                                              
> ==>  anyblok-io-xml (uninstalled),                                              
> ==>  model_authz (uninstalled),                                                 
> ==>  todolist (installed)]       

                                                                                
## Lister les **Model** existant.                                               

Même principe que pour les bloks, a la différence que seule les Model des bloks installé sont visibles.
                                                                                
> sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
> ==> In [1]: registry.System.Model.query().all().name                            
> ==> Out[1]:                                                                     
> ==> ['Model.System',                                                            
> ==>  'Model.System.Column',                                                     
> ==>  'Model.Authorization',                                                     
> ==>  'Model.Documentation.Blok',                                                
> ==>  'Model.System.Parameter',                                                  
> ==>  'Model.System.Cron',                                                       
> ==>  'Model.Documentation.Model',                                               
> ==>  'Model.System.Cache',                                                      
> ==>  'Model.Documentation.Model.Field',                                         
> ==>  'Model.System.Model',                                                      
> ==>  'Model.System.Sequence',                                                   
> ==>  'Model.System.Cron.Worker',                                                
> ==>  'Model.System.Blok',                                                       
> ==>  'Model.System.Cron.Job',                                                   
> ==>  'Model.System.Field',                                                      
> ==>  'Model.Documentation.Model.Attribute',                                     
> ==>  'Model.System.RelationShip',                                               
> ==>  'Model.Documentation']         

                                                                                
## Ajout du model **Todo**

La notion de Model est définit par l'ORM. Cependant pour les besoin d'AnyBlok la déclaration est wrappé. le but étant de n'utilisé que les Models des bloks installés.
                                                                                
### Créer le module **todo.py** dans le blok **todolist**                      

Avec votre éditeur préféré définissez le model **Todo**.

```python
from anyblok import Declarations                                                                                                               
from anyblok.column import Integer, String, Text, DateTime, Selection                                                                          
from datetime import datetime                                                                                                                  
                                                                            
                                                                            
@Declarations.register(Declarations.Model)                                                                                                     
class Todo:                                                                 
                                                                            
    id = Integer(primary_key=True)                                                                                                             
    title = String(nullable=False)                                                                                                             
    description = Text()                                                                                                                       
    creation_date = DateTime(default=datetime.now, nullable=False)                                                                             
    deadline_date = DateTime()                                                                                                                 
    ending_date = DateTime()                                                                                                                   
    cancelled_date = DateTime()                                                 
    step = Selection(selections=[('todo', 'Todo'), ('done', 'Done'),            
                                 ('canceled', 'Canceled')],                     
                     nullable=False, default='todo')                            
    complexity = Selection(selections=[('veasy', 'Very easy'),                  
                                       ('easy', 'Easy'),                        
                                       ('normal', 'Normal'),                    
                                       ('dificulte', 'Dificulte'),              
                                       ('vdificulte', 'Very dificulte')],       
                           nullable=False, default='normal')                    
    priority = Selection(selections=[('3', 'Low'),                              
                                     ('2', 'Normal'),                           
                                     ('1', 'Hight')],                           
                         nullable=False, default='2')  
```

Le decorateur registeur permet de déclaré qu'un model sera definit dans le blok. Ce model est définit par un class python classique. Pas besoin de metaclass ni d'hériter d'une class spécifique.
                             
                                                                                
### Déclarer le fichier todo dans le blok

Les modules python dans un blok doivent être chargé par AnyBlok. Pour cela il faut implementé la méthode **import_declaration_module** dans la class définissant le blok (ici BlokTodoList).

> anyblok_todolist/anyblok_todolist/todolist/__init__.py
                                                                                
```python                                                                              

...

class BlokTodoList(Blok):

    ...

    @classmethod
    def import_declaration_module(cls):
        from . import todo  # noqa

    @classmethod
    def reload_declaration_module(cls, reload):
        from . import todo
        reload(todo)

```

> **WARNING**: les ... indiques que le code existant n'est pas modifié, afin d'allégé je ne les ai pas recipié.

La methode **reload_declaration_module** est appelé lors d'une demande de rechargement du code python par l'application.

### Lister les models et fields

Vu que le blok est installé, Le model est alors disponible depuis l'interpreteur.

sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql
==> In [1]: registry.System.Model.query().all().name
==> Out[1]:
==> ['Model.System',
==>  'Model.System.Column',
==>  'Model.Authorization',
==>  'Model.Documentation.Blok',
==>  'Model.System.Parameter',
==>  'Model.System.Cron',
==>  'Model.Documentation.Model',
==>  'Model.System.Cache',
==>  'Model.Documentation.Model.Field',
==>  'Model.System.Model',
==>  'Model.System.Sequence',
==>  'Model.System.Cron.Worker',
==>  'Model.System.Blok',
==>  'Model.System.Cron.Job',
==>  'Model.System.Field',
==>  'Model.Documentation.Model.Attribute',
==>  'Model.System.RelationShip',
==>  'Model.Documentation',
==>  'Model.Todo']
==> In [2]: registry.System.Field.query().filter_by(model='Model.Todo').all().name
==> Out[2]:
==> ['cancelled_date',
==>  'title',
==>  'description',
==>  'complexity',
==>  'step',
==>  'priority',
==>  'creation_date',
==>  'id',
==>  'ending_date',
==>  'deadline_date']

### Lister le schéma

Depuis l'interpreteur le model exist dans la base de donnée mais pas depuis le client postgres. Simplement parce que la migration du blok lié a l'ajout du model n'a pas été commité par l'interpreteur. Pour le rendre disponible il faut soit:
* commiter la session: 
  ```python
  registry.commit()
  ```
* mettre a jour la base via un console script:
  > sandbox/bin/anyblok_updatedb --db-name todolist --db-driver-name postgresql --update-bloks todolist
                                                                                
> psql todolist                                                                   
> ==> \d todo                                                                     
>                         Table "public.todo"                                     
>      Column     |           Type           |                     Modifiers                     
> ----------------+--------------------------+---------------------------------------------------
>  cancelled_date | timestamp with time zone |                                    
>  title          | character varying(64)    | not null                           
>  description    | text                     |                                    
>  complexity     | character varying(64)    | not null                           
>  step           | character varying(64)    | not null                           
>  priority       | character varying(64)    | not null                           
>  creation_date  | timestamp with time zone | not null                           
>  id             | integer                  | not null default nextval('todo_id_seq'::regclass)
>  ending_date    | timestamp with time zone |                                    
>  deadline_date  | timestamp with time zone |                                    
>                                                                                 
> Indexes:                                                                        
>  "anyblok_pk_todo" PRIMARY KEY, btree (id)                                      

La base de donnée est cohérente avec le model définis. Merci SQLAlchemy ;)
                                                                                
### Enregister des données

Jusqu'a présent il n'y a eu que de la recherche et de la lecture de Model. Blok, Model et Field sont bien des models définis par AnyBlok.

Pour ajouter une nouvelle entité nous utilisons la method insert sur le Model. Ca correspond au ligne suivante pour SQLAlchemy

```python
session.add(Foo(bar=True))
session.flush()
```

est remplacé par:

```python
registry.Foo.insert(bar=True)
```

La methode **insert** n'existe pas dans le fichier de déclaration du model. Cette methode a été ajouté par AnyBlok

> sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
> ==> In [1]: registry.Todo.insert(title="My first todo entry in the todolist")   
> ==> Out[1]: <anyblok.model.todo at 0x111353d30>                                 
>                                                                                 
> ==> In [2]: registry.Todo.query().all()                                         
> ==> Out[2]: [<anyblok.model.todo at 0x111353d30>]                               
>                                                                                 
> ==> In [3]: registry.commit()           

                                                                               
## Amélioré le résultat affiché

L'ORM permet de dissocié les lignes de la table. Chaque ligne est représenté par une instance de class. Étant en python il est donc possible de définir un meilleur retoure que <anyblok.model.todo at 0x111353d30>.

```python

...

@Declarations.register(Declarations.Model)
class Todo:

    ...

    def __str__(self):                                                           
        return ('{self.step.label}: {self.title} '
                '[{self.priority.label} / {self.complexity.label}]').format(
            self=self)
                                                                                                             
    def __repr__(self):
        msg = ('<TodoList: title={self.title}, '
               'step={self.step.label}, '
               'creation date={self.creation_date}')

        if self.deadline_date:
            msg += ', deadline={self.deadline_date}'

        if self.step == 'canceled':
            msg += ', canceled date={self.cancelled_date}'
        elif self.step == 'done':
            msg += ', canceled date={self.ending_date}'
        else:
            msg += (', priority={self.priority.label}'
                    ', complexity={self.complexity.label}')

        msg += ' >'
        return msg.format(self=self)
```
                                                                                
Nous avons donc un résultat plus sexy.
                                                                                
> sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
> ==> In [1]: registry.Todo.query().all()                                         
> ==> Out[1]: [<TodoList: title=My first todo entry in the todolist, step=Todo, creation date=2016-05-11 10:50:50.293404+02:00, priority=Normal, complexity=Normal >]
> ==>                                                                             
> ==> In [2]: str(registry.Todo.query().first())                                  
> ==> Out[2]: 'Todo: My first todo entry in the todolist [Normal / Normal]'       
                                                                                
## Ajout d'un workflow

L'objectif de ce workflow est de fournir sur le modele des methodes haut niveau pour la gestion de la todolist.

### Ajout d'exception

Par convention les exceptions sont placé dans un module python qui n'est pas chargé par AnyBlok. Les exceptions sont donc importable a n'importe quel moment, même si le blok n'est pas installé.

Ajouter le fichier exceptions.py dans le blok
> anyblok_todolist/anyblok_todolist/todolist/exceptions.py

```python
class TodoException(Exception):                                             
    pass                                                                    
                                                                            
                                                                            
class TodoExceptionDone(TodoException):                                     
    pass                                                                    
                                                                            
                                                                            
class TodoExceptionCanceled(TodoException):                                    
    pass                                                                    
```

Ici le but étant d'avoir des exceptions de base lié a notre workflow.

### Modification du modele pour ajouter les méthodes haut niveaux

Il suffit de modifier le modele existant est d'ecrire les commande qui agisse sur une instance du modele.

```python
                                                                            
...                                                                         
                                                                            
@Declarations.register(Declarations.Model)                                       
class Todo:                                                                    
                                                                            
    ...                                                                     
                                                                            
                                                                            
    def do(self):                                                               
        if self.step != 'todo':                                                 
            raise TodoExceptionDone("You can change step %r to 'Done'" %        
                                    self.step.label)                            
        self.step = 'done'                                                      
        self.ending_date = datetime.now()                                       
                                                                            
    def cancel(self):                                                           
        if self.step != 'todo':                                                 
            raise TodoExceptionCanceled("You can change step %r to 'Canceled'"  
                                        % self.step.label)                      
        self.step = 'canceled'                                                    
        self.ending_date = datetime.now()                                   
```                                                           

Les methodes sont disponibles dans l'interpreter.

> sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
> ==> In [1]: todo = registry.Todo.query().first()                                
>                                                                                 
> ==> In [2]: todo                                                                
> ==> Out[2]: <TodoList: title=My first todo entry in the todolist, step=Todo, creation date=2016-05-11 10:50:50.293404+02:00, priority=Normal, complexity=Normal >
>                                                                                 
> ==> In [3]: todo.do()                                                           
>                                                                                 
> ==> In [4]: todo                                                                
> ==> Out[4]: <TodoList: title=My first todo entry in the todolist, step=Done, creation date=2016-05-11 10:50:50.293404+02:00, canceled date=2016-05-11 10:22:26.515175+01:00 >
>                                                                                 
> ==> In [5]: todo.cancel()                                                       
>     --------------------------------------------------------------------------- 
>     TodoExceptionCanceled                     Traceback (most recent call last) 
>     /Users/jssuzanne/anybox/tuto_anyblok/sandbox/lib/python3.5/site-packages/anyblok/scripts.py in <module>()
>     ----> 1 todo.cancel()                                                       
>                                                                                 
>     /Users/jssuzanne/anybox/tuto_anyblok/anyblok_todolist/anyblok_todolist/todolist/todo.py in cancel(self)
>          63         if self.step != 'todo':                                     
>          64             raise TodoExceptionCanceled("You can change step %r to 'Canceled'"
>     ---> 65                                         % self.step.label)          
>          66         self.step = 'cancel'                                        
>          67         self.ending_date = datetime.now()                           
>                                                                                 
>     TodoExceptionCanceled: You can change step 'Done' to 'Canceled'             
                                                                     
                                                                                 
## Ajout de test unitaire

AnyBlok fournit un console script pour lancé les tests unitaires il impose un certain nombre de contrainte. Les tests doivent être placé dans un répertoire **tests** a la racine du blok.
                                                                                
> todolist                                                                        
> .. __init__.py                                                                  
> .. exceptions.py                                                                
> .. todo.py                                                                      
> .. tests                                                                        
> .. .. __init__.py                                                               
> .. .. test_todo.py                                                              

Modifier le fichier test_todo.py

```python
from anyblok.tests.testcase import BlokTestCase                             
from ..exceptions import TodoException                                      
                                                                            
                                                                            
class TestTodo(BlokTestCase):                                                                                                                  
                                                                            
    def test_do(self):                                                      
        todo = self.registry.Todo.insert(title='Test')                      
        self.assertEqual(todo.step, 'todo')                                 
        todo.do()                                                           
        self.assertEqual(todo.step, 'done')                                 
                                                                            
    def test_do_with_bad_step(self):                                        
        todo = self.registry.Todo.insert(title='Test')                      
        self.assertEqual(todo.step, 'todo')                                 
        todo.step = 'canceled'                                              
        with self.assertRaises(TodoException):                              
            todo.do()                                                       
                                                                            
     def test_cancel(self):                                                 
         todo = self.registry.Todo.insert(title='Test')                     
         self.assertEqual(todo.step, 'todo')                                
         todo.cancel()                                                      
         self.assertEqual(todo.step, 'canceled')                            
                                                                            
     def test_cancel_with_bad_step(self):                                   
         todo = self.registry.Todo.insert(title='Test')                     
         self.assertEqual(todo.step, 'todo')                                
         todo.step = 'done'                                                 
         with self.assertRaises(TodoException):                             
             todo.cancel()                                                  
```
                                                                                
Lancer les tests unitaires.
                                                                                
> jssuzanne:tuto_anyblok jssuzanne$ sandbox/bin/anyblok_nose --db-name todolist --db-driver-name postgresql --selected-bloks todolist
> Load config file '/Library/Application Support/AnyBlok/conf.cfg'                
> Load config file '/Users/jssuzanne/Library/Application Support/AnyBlok/conf.cfg'
> ....                                                                            
> ----------------------------------------------------------------------          
> Ran 4 tests in 0.880s                                                           
>                                                                                 
> OK                                                                              

Félicitation, Vous avez votre propre blok, avec un Model et des methodes haut niveau. Le tout sous la couverture de test unitaire.
