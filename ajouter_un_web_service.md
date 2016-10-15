# Ajouter un web service

Nous utilisons **Pyramid** pour ajouter une api Rest et un petit client.

## Ajouter un nouveau blok

### Modifier le setup du paquet python

Il faut ajouter dans l'**entry_point** le nouveau blok. Et ne pas oublié la dépendnce a **anyblok_pyramid**.

**anyblok_pyramid** a, comme son nom l'indique une dépendance vers **Pyramid**

```python

...

required = [
    ...
    'anyblok_pyramid',
]

setupt(
    ...
    entry_points={
        'bloks': [
            ...
            'todolist=anyblok_todolist.todolist:BlokTodoList',
            'todolist-client=anyblok_todolist.client:BlokTodoListClient',
        ],
    },
)
```

### Ajouter le blok BlokTodoListClient

Au même niveau que le blok todolist nous ajoutons le repertoire client qui porte le blok BlokTodoListClient dans sont fichier __init__.py

> .. anyblok_todolist
> .. ...
> .. .. todolist
> .. .. ...
> .. .. client
> .. .. .. __init__.py

Ici en plus de la version nous allons ajouter une notion de dépendence entre blok symbolisé par l'attribut **required**. Ca indique que le todolist sera installé avec se bloc.

```python
from anyblok.blok import Blok


class BlokTodoListClient(Blok):
   version = '0.1.0'

   required = ['todolist']
```

### Mis à jour du paquet dans l'environnement python.

Nous devons mettre a jour notre environnement pour que le nouveau **entry_point**, sinon le nouveau bloc ne sera pas disponible dans la liste des bloks car non connu.

> cd anyblok_todolist
> ../sandbox/bin/python setup.py develop
> cd ..

### Installer le nouveau blok

Cette commande n'a plus de secret pour vous
                                                                                
> sandbox/bin/anyblok_updatedb --db-name todolist --db-driver-name postgresql --install-bloks todolist-client

### Lister les bloks installés

> sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql
> ==> In [1]: registry.System.Blok.query().all()
> ==> Out[1]:
> ==> [anyblok-core (installed),
> ==>  anyblok-io (uninstalled),
> ==>  anyblok-io-csv (uninstalled),
> ==>  anyblok-io-xml (uninstalled),
> ==>  model_authz (uninstalled),
> ==>  todolist (installed),
> ==>  todolist-client (installed)]

### Démarrer le serveur

Seulement pour le developpement, car ce console script utilise le serveur wsgi de python. Pour une exploitation plus robuste il faut utiliser **gunicorn** ou **pyserve**

> sandbox/bin/anyblok_pyramid --db-name todolist --db-driver-name postgresql

### Faire une première requête

Pour cela il suffit soit d'utiliser:
* un navigteur
* wget ou curl

> wget localhost:5000/
> --2016-05-11 14:32:15--  http://localhost:5000/
> Résolution de localhost (localhost)… 127.0.0.1, ::1
> Connexion à localhost (localhost)|127.0.0.1|:5000… connecté.
> requête HTTP transmise, en attente de la réponse… 404 Not Found
> 2016-05-11 14:32:15 erreur 404 : Not Found.
                                                                                
Sur le serveur vous pouvez voir les logs associés

> 127.0.0.1 - - [11/May/2016 14:32:15] "GET / HTTP/1.1" 404 153

Ici nous recevons un 404, ce qui est normal car aucune route n'a été définit

## Convertion DateTime en string

Avant de définir les routes nous allons commencé par indiqué a **Pyramid** comment représenté un **DateTime**. En effet les api retournent des json

Dans le fichier:
> anyblok_todolist/anyblok_todolist/client/pyramid.py

```python
from pyramid.renderers import JSON
from datetime import datetime
import pytz
import time


def datetime_adapter(obj, request):
   if obj is not None:
       if obj.tzinfo is None:
           timezone = pytz.timezone(time.tzname[0])
           obj = timezone.localize(obj)

   return obj.isoformat()


def json_data_adapter(config):
   json_renderer = JSON()
   json_renderer.add_adapter(datetime, datetime_adapter)
   config.add_renderer('json', json_renderer)
```

Dans le fichier de déclaration du blok:
> anyblok_todolist/anyblok_todolist/client/__init__.py

```python

...
from .pyramid import json_data_adapter


class BlokTodoListClient(Blok):
    ...

    @classmethod
    def pyramid_load_config(cls, config):
        json_data_adapter(config)
```

La methode **pyramid_load_config**

2.1) In __init__.py of the blok todolist-client                                 
                                                                                
                                                                                
3) A Rest API                                                                   
                                                                                
3.1) add module files in the blok                                               
                                                                                
client                                                                          
.. __init__.py                                                                  
.. views                                                                        
.. .. __init__.py                                                               
.. .. todolist.py                                                               
.. .. todo.py                                                                   
.. .. step.py                                                                   
.. tests                                                                        
.. .. __init__.py                                                               
.. .. test_todolist.py                                                          
.. .. test_todo.py                                                              
.. .. test_step.py                                                              
                                                                                
3.2) Define the route of the API                                                
                                                                                
* todolist: read all, new                                                       
* todo: read one, update, delete                                                
* step: change the step                                                         
                                                                                
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    class BlokTodoListClient(Blok):                                             
                                                                                
        ...                                                                     
                                                                                
        @classmethod                                                                
        def pyramid_load_config(cls, config):                                       
            json_data_adapter(config)                                               
            config.add_route('todolist', '/todolist/')                              
            config.add_route('todo', '/todolist/{id}')                          
            config.add_route('step', '/todolist/{id}/{step}')                   
            config.scan(cls.__module__ + '.views')                              
                                                                                
3.3) Add views                                                                  
                                                                                
* GET /todolist/: list all entries                                              
* GET /todolist/{id}: read on entry                                             
* POST /todolist/: create a new entry                                           
* PUT /todolist/{id}: update one entry                                          
* DELETE /todolist/{id}: remove an existing entry                               
* POST /todolist/{id}/{step}: update the step for l'id                          
                                                                                
3.3.1) Add GET todolist                                                         
                                                                                
can take parameter (text search, step, complexity, priority)                    
return JSON dict {title, step, complexity, priority}                            
 
                                                                                
3.3.1.1) Add view                                                               
                                                                                
in views/todolist.py::                                                          
                                                                                
    from pyramid.view import view_defaults, view_config                         
    from anyblok_pyramid import current_blok                                    
    from sqlalchemy import or_                                                  
    import sqlalchemy                                                           
                                                                                
                                                                                
    upper = sqlalchemy.func.upper                                               
                                                                                
                                                                                
    @view_defaults(installed_blok=current_blok(),                               
                   route_name='todolist',                                       
                   renderer="json")                                             
    class TodoList:                                                             
                                                                                
        def __init__(self, request):                                            
            self.request = request                                              
            self.registry = request.anyblok.registry                            
                                                                                
        @view_config(request_method="GET")                                      
        def list_all_entries(self):                                             
            params = dict(self.request.params)                                  
            Todo = self.registry.Todo                                           
            query = Todo.query()                                                
            if 'filter_txt' in params:                                          
                filter_txt = '%' + params.pop('filter_txt').upper() + '%'       
                query = query.filter(                                           
                    or_(upper(Todo.title).ilike(filter_txt),                    
                        upper(Todo.description).ilike(filter_txt)))             
                                                                                
            for field, value in params.items():                                 
                query = query.filter(getattr(Todo, field) == value)             
                                                                                
            query = query.order_by(Todo.priority, Todo.creation_date)           
            result = query.all()                                                
                                                                                
            if not result:                                                      
                return []                                                       
                                                                                
            return result.to_dict('title', 'step', 'priority', 'complexity')    
 
 3.3.1.2) test the view                                                          
                                                                                
wget localhost:5000/todolist/                                                   
[{id: 1, "step": "todo", "title": "My first todo entry in the todolist", "complexity": "normal", "priority": "2"}]
                                                                                
on the serveur                                                                  
127.0.0.1 - - [11/May/2016 18:06:14] "GET /todolist/ HTTP/1.1" 200 107          
  
  3.3.1.3) add unit tests                                                         
                                                                                
::                                                                              
                                                                                
    from anyblok_pyramid.tests.testcase import PyramidBlokTestCase              
    from random import randint, choice                                          
                                                                                
                                                                                
    class TestTodoList(PyramidBlokTestCase):                                    
                                                                                
        def add_random_todo(self):                                              
            entries = []                                                        
            for x in range(randint(0, 100)):                                    
                e = self.registry.Todo.insert(title='Toto %s' % x,              
                                              description='Desc %s' % x,        
                                              step=choice(['todo',              
                                                           'done',              
                                                           'canceled']),        
                                              priority=choice(['1', '2', '3']), 
                                              complexity=choice(['veasy',       
                                                                 'easy',        
                                                                 'normal',      
                                                                 'dificulte',   
                                                                 'vdificulte']))
                entries.append(e)                                               
                                                                                
            return entries                                                      
                                                                                
        def test_list_all(self):                                                
            self.add_random_todo()                                              
            res = self.webserver.get('/todolist/', status=200)                  
            self.assertEqual(self.registry.Todo.query().count(),                
                             len(res.json_body))                                
                                                                                
        def test_list_schema(self):                                             
            entries = self.add_random_todo()                                    
            entry = entries[0]                                                  
            res = self.webserver.get('/todolist/?id=%d' % entry.id, status=200) 
            self.assertEqual(res.json_body[0],                                  
                             {'id': entry.id,                                   
                              'title': entry.title,                             
                              'step': entry.step,                               
                              'priority': entry.priority,                       
                              'complexity': entry.complexity})                  
                                                                                
        def test_list_filter_by_name(self):                                     
            Todo = self.registry.Todo                                           
            self.add_random_todo()                                              
            res = self.webserver.get('/todolist/?filter_txt=toto', status=200)  
            self.assertEqual(                                                   
                Todo.query().filter(Todo.title.ilike('Toto%')).count(),         
                len(res.json_body))                                             
                                                                                
        def test_list_filter_by_desc(self):                                     
            Todo = self.registry.Todo                                           
            self.add_random_todo()                                              
            res = self.webserver.get('/todolist/?filter_txt=desc', status=200)  
            self.assertEqual(                                                   
                Todo.query().filter(Todo.description.ilike('Desc%')).count(),   
                
        def test_list_filter_by_step(self):                                     
            Todo = self.registry.Todo                                           
            self.add_random_todo()                                              
            res = self.webserver.get('/todolist/?step=todo', status=200)        
            self.assertEqual(                                                   
                Todo.query().filter(Todo.step == 'todo').count(),               
                len(res.json_body))                                             
                                                                                
        def test_list_filter_by_complexity(self):                               
            Todo = self.registry.Todo                                           
            self.add_random_todo()                                              
            res = self.webserver.get('/todolist/?complexity=normal', status=200)
            self.assertEqual(                                                   
                Todo.query().filter(Todo.complexity == 'normal').count(),       
                len(res.json_body))                                             
                                                                                
        def test_list_filter_by_priority(self):                                 
            Todo = self.registry.Todo                                           
            self.add_random_todo()                                              
            res = self.webserver.get('/todolist/?priority=2', status=200)       
            self.assertEqual(                                                   
                Todo.query().filter(Todo.priority == '2').count(),              
                len(res.json_body))                                             
                                                                                
3.3.2) Add GET todo                                                             
                                                                                
3.3.2.1) Add view                                                               
                                                                                
::                                                                              
                                                                                
    from pyramid.view import view_defaults, view_config                         
    from anyblok_pyramid import current_blok                                    
    from pyramid.httpexceptions import HTTPNotFound                             
                                                                                
                                                                                
    @view_defaults(installed_blok=current_blok(),                               
                   route_name='todo',                                           
                   renderer="json")                                             
    class Todo:                                                                 
                                                                                
        def __init__(self, request):                                            
            self.request = request                                              
            self.registry = request.anyblok.registry                            
                                                                                
        @view_config(request_method="GET")                                      
            def list_all_entries(self):                                         
            id = self.request.matchdict.get('id')                               
            if id is None:                                                      
                return HTTPNotFound()                                           
                                                                                
            todo = self.registry.Todo.query().get(int(id))                      
            if todo is None:                                                    
                return HTTPNotFound()                                           
                                                                                
            return todo.to_dict()                                               
                
                                                                                
3.3.2.2) Add unittest                                                           
                                                                                
::                                                                              
                                                                                
    from anyblok_pyramid.tests.testcase import PyramidBlokTestCase              
    import pytz                                                                 
    import time                                                                 
                                                                                
                                                                                
    def format_datetime(dt):                                                         
        if dt is None:                                                               
            return None                                                              
                                                                                
        if dt.tzinfo is None:                                                        
            timezone = pytz.timezone(time.tzname[0])                                 
            dt = timezone.localize(dt)                                               
                                                                                
        return dt.isoformat()                                                   
                                                                                
                                                                                
    class TestTodo(PyramidBlokTestCase):                                        
                                                                                
        def assertGetValue(self, res, todo):                                    
            self.assertEqual(res.json_body, {                                   
                            'id': todo.id,                                      
                            'title': todo.title,                                
                            'description': todo.description,                    
                            'creation_date': format_datetime(todo.creation_date),
                            'deadline_date': format_datetime(todo.deadline_date),
                            'ending_date': format_datetime(todo.ending_date),   
                            'cancelled_date': format_datetime(todo.cancelled_date),
                            'step': todo.step,                                  
                            'complexity': todo.complexity,                      
                            'priority': todo.priority,                          
            })                                                                  
                                                                                
        def test_get_todo(self):                                                
            todo = self.registry.Todo.insert(title='Todo')                      
            res = self.webserver.get('/todolist/%d' % todo.id, status=200)      
            self.assertGetValue(res, todo)                                      
                                                                                
        def test_get_done(self):                                                
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.do()                                                           
            res = self.webserver.get('/todolist/%d' % todo.id, status=200)      
            self.assertGetValue(res, todo)                                      
                                                                                
        def test_get_cancelled(self):                                           
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.cancel()                                                       
            res = self.webserver.get('/todolist/%d' % todo.id, status=200)      
            self.assertGetValue(res, todo)                                      
                                                                                
3.3.3) Add create view                                                          

                                                                               
3.3.3.1) add view                                                               
                                                                                
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    class TodoList:                                                             
                                                                                
        ...                                                                     
                                                                                
        @view_config(request_method="POST")                                     
        def create_new_entry(self):                                             
            params = dict(self.request.params)                                  
            todo = self.registry.Todo.insert(**params)                          
            return todo.to_dict()                                               
                                                                                
3.3.3.2) add unit test                                                          
                                                                                
::                                                                              
                                                                                
    import pytz                                                                 
    import time                                                                 
                                                                                
                                                                                
    def format_datetime(dt):                                                    
        if dt is None:                                                          
            return None                                                         
                                                                                
        if dt.tzinfo is None:                                                   
            timezone = pytz.timezone(time.tzname[0])                            
            dt = timezone.localize(dt)                                          
                                                                                
        return dt.isoformat()                                                   
                                                                                
                                                                                
    class TestTodoList(PyramidBlokTestCase):                                    
                                                                                
        ...                                                                     
                                                                                
        def assertGetValue(self, res, todo):                                    
            self.assertEqual(res, {                                             
                'id': todo.id,                                                  
                'title': todo.title,                                            
                'description': todo.description,                                
                'creation_date': format_datetime(todo.creation_date),           
                'deadline_date': format_datetime(todo.deadline_date),           
                'ending_date': format_datetime(todo.ending_date),               
                'cancelled_date': format_datetime(todo.cancelled_date),         
                'step': todo.step,                                              
                'complexity': todo.complexity,                                  
                'priority': todo.priority,                                      
            })                                                                  
                                                                                
        def test_post(self):                                                    
            res = self.webserver.post('/todolist/', {'title': 'Test'}, status=200)
            res = res.json_body                                                 
            todo = self.registry.Todo.query().get(int(res['id']))               
            self.assertGetValue(res, todo)                                      
                                                                                
3.3.4) Add update view                                                          
                                                                                
3.3.4.1) add view                                                               
                                                                                
::                                                                              
                                                                                
    ...                                                                         
    from pyramid.httpexceptions import HTTPNotFound, HTTPForbidden              
                                                                                
                                                                                
    @view_defaults(installed_blok=current_blok(),                               
                   route_name='todo',                                           
                                  renderer="json")                              
    class Todo:                                                                 
                                                                                
        ...                                                                     
                                                                                
        @view_config(request_method="PUT")                                      
        def update_existing_entry(self):                                        
            id = self.request.matchdict.get('id')                               
            if id is None:                                                      
                return HTTPNotFound()                                           
                                                                                
            params = dict(self.request.params)                                  
            todo = self.registry.Todo.query().get(int(id))                      
                                                                                
            if todo.step != 'todo':                                             
                return HTTPForbidden()                                          
                                                                                
            todo.update(**params)                                               
            return todo.to_dict()                                               
                                                                                 
3.3.4.2) add unit test                                                          
                                                                                
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    class TestTodo(PyramidBlokTestCase):                                        
                                                                                
        ...                                                                     
                                                                                
        def test_put(self):                                                     
            todo = self.registry.Todo.insert(title='Todo')                      
            res = self.webserver.put('/todolist/%d' % todo.id,                  
                                     {'priority': '1'},                         
                                     status=200)                                
            self.assertGetValue(res, todo)                                      
            self.assertEqual(todo.priority, '1')                                
                                                                                
        def test_put_done(self):                                                
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.do()                                                           
            self.webserver.put('/todolist/%d' % todo.id,                        
                               {'priority': '1'},                               
                               status=403)                                      
            self.assertNotEqual(todo.priority, '1')                             
                                                                                
        def test_put_cancelled(self):                                           
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.cancel()                                                       
            self.webserver.put('/todolist/%d' % todo.id,                        
                               {'priority': '1'},                               
                               status=403)                                      
            self.assertNotEqual(todo.priority, '1')                             
                                                                               
3.3.5) Add delete view                                                          
                                                                                
3.3.5.1) add view                                                               
                                                                                
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    class Todo:                                                                 
                                                                                
        ...                                                                     
                                                                                
        @view_config(request_method="DELETE")                                   
        def delete_entry(self):                                                 
            id = self.request.matchdict.get('id')                               
            if id is None:                                                      
                return HTTPNotFound()                                           
                                                                                
            todo = self.registry.Todo.query().get(int(id))                      
            if todo.step != 'todo':                                             
                return HTTPForbidden()                                          
                                                                                
            todo.delete()                                                       
            return {'id': todo.id}                                              
                                                                                
3.3.5.2) add unit test                                                          
                                                                                
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    class TestTodo(PyramidBlokTestCase):                                        
                                                                                
        ...                                                                     
                                                                                
        def test_delete(self):                                                  
            todo = self.registry.Todo.insert(title='Todo')                      
            res = self.webserver.delete('/todolist/%d' % todo.id, status=200)   
            self.assertEqual(res.json_body, {'id': todo.id})                    
            query = self.registry.Todo.query().filter_by(id=todo.id)            
            self.assertEqual(query.count(), 0)                                  
                                                                                
        def test_delete_done(self):                                             
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.do()                                                           
            self.webserver.delete('/todolist/%d' % todo.id, status=403)         
            query = self.registry.Todo.query().filter_by(id=todo.id)            
            self.assertEqual(query.count(), 1)                                  
                                                                                
        def test_delete_cancelled(self):                                        
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.cancel()                                                       
            self.webserver.delete('/todolist/%d' % todo.id, status=403)         
            query = self.registry.Todo.query().filter_by(id=todo.id)            
            self.assertEqual(query.count(), 1)                                  
                                                                                
3.3.6) add change step                                                          
   
                                                                               
3.3.6.1) add view                                                               
                                                                                
::                                                                              
                                                                                
    from pyramid.view import view_defaults, view_config                         
    from anyblok_pyramid import current_blok                                    
    from pyramid.httpexceptions import HTTPNotFound, HTTPForbidden              
                                                                                
                                                                                
    @view_defaults(installed_blok=current_blok(),                               
                   route_name='step',                                           
                   renderer="json")                                             
    class Todo:                                                                 
                                                                                
        def __init__(self, request):                                            
            self.request = request                                              
            self.registry = request.anyblok.registry                            
                                                                                
        @view_config(request_method="POST")                                     
        def update_existing_entry(self):                                        
            id = self.request.matchdict.get('id')                               
            step = self.request.matchdict.get('step')                           
            if id is None or step is None:                                      
                return HTTPNotFound()                                           
                                                                                
            todo = self.registry.Todo.query().get(int(id))                      
            if todo.step != 'todo':                                             
                return HTTPForbidden()                                          
                                                                                
            getattr(todo, step)()                                               
            return todo.to_dict()                                               
.3.6.2) add unit test                                                          
                                                                                
::                                                                              
                                                                                
    from anyblok_pyramid.tests.testcase import PyramidBlokTestCase              
    import pytz                                                                 
    import time                                                                 
                                                                                
                                                                                
    def format_datetime(dt):                                                    
        if dt is None:                                                          
            return None                                                         
                                                                                
        if dt.tzinfo is None:                                                   
            timezone = pytz.timezone(time.tzname[0])                            
            dt = timezone.localize(dt)                                          
                                                                                
        return dt.isoformat()                                                   
                                                                                
                                                                                
    class TestStep(PyramidBlokTestCase):                                        
                                                                                
        def assertGetValue(self, res, todo):                                    
            self.assertEqual(res.json_body, {                                   
                'id': todo.id,                                                  
                'title': todo.title,                                            
                'description': todo.description,                                
                'creation_date': format_datetime(todo.creation_date),           
                'deadline_date': format_datetime(todo.deadline_date),           
                'ending_date': format_datetime(todo.ending_date),               
                'cancelled_date': format_datetime(todo.cancelled_date),         
                'step': todo.step,                                              
                'complexity': todo.complexity,                                  
                'priority': todo.priority,                                      
            })                                                                  
                                                                                
        def test_do(self):                                                      
            todo = self.registry.Todo.insert(title='Todo')                      
            res = self.webserver.post('/todolist/%d/do' % todo.id, status=200)  
            self.assertGetValue(res, todo)                                      
                                                                                
        def test_do_when_already_done(self):                                    
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.do()                                                           
            self.webserver.post('/todolist/%d/do' % todo.id, status=403)        
                                                                                
        def test_do_when_already_cancelled(self):                               
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.cancel()                                                       
            self.webserver.post('/todolist/%d/do' % todo.id, status=403)        
                                                                                
        def test_cancel(self):                                                  
            todo = self.registry.Todo.insert(title='Todo')                      
            res = self.webserver.post('/todolist/%d/cancel' % todo.id, status=200)
            self.assertGetValue(res, todo)                                      
                                                                                
        def test_cancel_when_already_done(self):                                
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.do()                                                           
            self.webserver.post('/todolist/%d/cancel' % todo.id, status=403)    
                                                                                
        def test_cancel_when_already_cancelled(self):                           
            todo = self.registry.Todo.insert(title='Todo')                      
            todo.cancel()                                                       
            self.webserver.post('/todolist/%d/cancel' % todo.id, status=403)    
                                                                                
Great, you have a RestFul API for a web service                                   
                                               
                                                                               
3.4) Add client for display todolist                                            
                                                                                
3.4.1) Display Selection value                                                  
                                                                                
3.4.1.1) Add route                                                              
                                                                                
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    @classmethod                                                                
    def pyramid_load_config(cls, config):                                       
        ...                                                                     
        config.add_route('properties', '/todolist/properties/')                 
        config.scan(cls.__module__ + '.views')                                  
                                                                                
3.4.1.2) add view                                                               
                                                                                
::                                                                              
                                                                                
    from pyramid.view import view_defaults, view_config                         
    from anyblok_pyramid import current_blok                                    
                                                                                
                                                                                
    @view_defaults(installed_blok=current_blok(),                               
                   route_name='properties',                                     
                   renderer="json")                                             
    class Properties:                                                           
                                                                                
        def __init__(self, request):                                            
            self.request = request                                              
            self.registry = request.anyblok.registry                            
                                                                                
        @view_config(request_method="GET")                                      
        def get_properties(self):                                               
            res = {}                                                            
            Column = self.registry.System.Column                                
            columns = Column.query().filter(Column.ftype == 'Selection',        
                                            Column.model == 'Model.Todo').all() 
                                                                                
            for column in columns:                                              
                col = getattr(self.registry.get(column.model), column.name)     
                res[column.name] = col.type.selections                          
                                                                                
            return res                                                          
                                                                                
3.4.1.3) Test the path                                                          
                                                                                
wget localhost:5000/todolist/properties/                                        
{"complexity": {"normal": "Normal", "veasy": "Very easy", "easy": "Easy", "vdificulte": "Very dificulte", "dificulte": "Dificulte"}, "priority": {"1": "Hight", "2": "Normal", "3": "Low"}, "step": {"done": "Done", "todo": "Todo", "canceled": "Canceled"}}
                                                                                
                                                                               
3.4.1.4) Add unit test                                                          
                                                                                
::                                                                              
                                                                                
    from anyblok_pyramid.tests.testcase import PyramidBlokTestCase              
                                                                                
                                                                                
    class TestProperties(PyramidBlokTestCase):                                  
                                                                                
        def test_get(self):                                                     
            res = self.webserver.get('/todolist/properties/', status=200)       
            self.assertEqual(res.json_body, {                                   
                "complexity": {                                                 
                    "normal": "Normal",                                         
                    "veasy": "Very easy",                                       
                    "easy": "Easy",                                             
                    "vdificulte": "Very dificulte",                             
                    "dificulte": "Dificulte"},                                  
                "priority": {"1": "Hight", "2": "Normal", "3": "Low"},          
                "step": {"done": "Done", "todo": "Todo", "canceled": "Canceled"}})
                                                                                
3.4.2) Define a static directory                                                
                                                                                
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    class BlokTodoListClient(Blok):                                             
                                                                                
        ...                                                                     
                                                                                
        static_paths = ['client']                                               
                                                                                
add client directory in the blok, it is not a python module                     
                                                                                
              
