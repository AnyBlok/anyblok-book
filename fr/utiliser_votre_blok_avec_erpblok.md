# Utiliser votre Blok avec ERPBlok

ERPBlok est comme son nom l'indique un ERP basé sur AnyBlok. Notre todolist est séparé du reste des modeles de l'ERP mais pour notre example ce n'est pas important.

## Installer ERPBlok

Actuellement ERPBlok est en cour de developpement, il n'a pas de dépos sur le pypi il faut donc allez le chercher sur github

```
git clone https://github.com/ERPBlok/ERPBlok.git
cd erpblok
../sandbox/bin/python setup.py install
cd ..
sandbox/bin/anyblok_pyramid --db-name todolist --db-driver-name postgresql
```

A partir d'un navigateur, connectez vous a l'ERP:
```
localhost:5000/
```

Par défaut les indentifiants de connexion sont:
* login: admin
* mot de passe: admin

Vous avez donc une application ERP et une application todolist en même temps. Cependant la todolist n'est pas accessible depuis l'ERP.


## Ajouter un nouveau blok

Le but de ce blok est de faire la liaison entre les bloks existant et l'ERP, Il s'agit principalement de configuration pour ajouter des menus.

### Ajouter un nouveau blok

La suite est déjà connu, nous allons ajouter un nouveau **entry_point** dans le fichier setup.

```python
...

setupt(
    ...
    entry_points={
        'bloks': [
            ...
            'todolist=anyblok_todolist.todolist:BlokTodoList',
            'todolist-client=anyblok_todolist.client:BlokTodoListClient',
            'erpblok-todolist=anyblok_todolist.erpblok_todolist:BlokTodoListERPBlok',
        ],
    },
)
```

Nous n'ajoutons pas de dépendance vers ERPBlok. Simplement parce le system de dépendance AnyBlok permettra de ne pas installer un blok lié avec ERPBlok si celui-ci n'est pas installé. Ainsi notre package reste installable même sans ERPBlok.

Nous allons ajouter le blok au même niveau que les bloks existants
```
.. anyblok_todolist
.. ...
.. .. todolist
.. .. ...
.. .. client
.. .. ...
.. .. erpblok_todolist
.. .. .. __init__.py
```

Dans le fichier
> anyblok_todolist/erpblok_todolist/__init__.py

```python
from anyblok.blok import Blok


class BlokTodoListERPBlok(Blok):
   version = '0.1.0'

   required = ['todolist-client', 'erpblok-core']
```

### Metter a jour le paquet python dans l'environnement

Même principe que la dernière fois, il faut que le blok soit dans l'environnement pour qu'il soit visible.

```
cd anyblok_todolist
../sandbox/bin/python setup.py develop
cd ..
```

### Installer le nouveau blok

```
sandbox/bin/anyblok_updatedb --db-name todolist --db-driver-name postgresql --install-bloks erpblok-todolist
sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql
==> In [1]: registry.System.Blok.query().all()
==> Out[1]:
==> [anyblok-core (installed),
==>  anyblok-io (installed),
==>  anyblok-io-csv (uninstalled),
==>  anyblok-io-xml (installed),
==>  erpblok-blok-manager (uninstalled),
==>  erpblok-web-client (installed),
==>  erpblok-core (installed),
==>  erpblok-debug (uninstalled),
==>  erpblok-demo (uninstalled),
==>  todolist (installed),
==>  erpblok-todolist (installed),
==>  model_authz (uninstalled),
==>  todolist-client (installed)]
```

Votre blok est installé il ne reste plus qu'a ajouter la configuration.

## Ajouter les menus

Dans ERPBlok les fonctionnalités sont régroupé sous la notion d'espace fonctionnel (Space) chaque espace possède sont ensemble de menu. Il est possible qu'un espace ne soit qu'une fonctionnalité sans aucun menu.

### Ajouter une nouvelle catégorie d'espace fonctionnel.

les catégories regroupes les espaces fonctionnel dans le menu général en haut a gauche.

dans le fichier:
> anyblok_todolist/erpblok_todolist/spaces.category.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<records>
    <record external_id="todolist_space_category">
        <field name="label">Todolist</field>
    </record>
</records>
```

### Ajouter un nouvel espace fonctionnel.

Notre espace n'aura pas de menu, il n'y aura qu'une action avec des vue liste / formulaire.

dans le fichier:
> anyblok_todolist/erpblok_todolist/space.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<records>
    <record external_id="setting_space_todolist">
        <field name="label">Todolist</field>
        <field name="icon">fi-database</field>
        <field name="description">ERPblok classical view for todolist</field>
        <field name="category" external_id="todolist_space_category" />
        <field name="default_action" external_id="action_setting_todolist">
            <field name="model">Model.Todo</field>
            <field name="label">Todolist</field>
            <field name="add_delete">1</field>
            <field name="add_new">1</field>
            <field name="add_edit">1</field>
        </field>
    </record>
</records>
```

### Charger la configuration dans le blok

Dans le fichier
> anyblok_todolist/erpblok_todolist/__init__.py

```python
...

class BlokTodoListERPBlok(Blok):
    ...

    def update(self, latest_version):
        super(BlokTodoListERPBlok, self).update(latest_version)
        self.import_file('xml', 'Model.Web.Space.Category',
                         'spaces.category.xml')
        self.import_file('xml', 'Model.Web.Space', 'space.xml')
```

La méthode **update** est appelé lors de l'installation ou de la mis à jour du blok. Dans le cas d'une modification de configuration la mis à jour est obligatoire.

```
sandbox/bin/anyblok_updatedb --db-name todolist --db-driver-name postgresql --update-bloks erpblok-todolist
```

## Définisser les vues liste et formulaire.

Actuellement les vues affiché sont créé par ERPBlok. Elle ne sont pas sexy. Nous allons donc les modifier.

Dans le fichier
> anyblok_todolist/erpblok_todolist/__init__.py

```python
...

class BlokTodoListERPBlok(Blok):
    ...

    views = [
        'todolist.tmpl',
    ]
```

l'attributs views permet de lister les fichiers de template, les vues étant des templates.

Dans le fichier
> anyblok_todolist/erpblok_todolist/todolist.tmpl

```xml
<templates>
    <template id="AnyBlokTodoForm">
       <div writable-only-if="fields.step == 'todo'">
           <div class="row">
               <div class="columns medium-8 large-10">
                   <field name="title" />
               </div>
               <div class="columns medium-4 large-2">
                   <button data-function="rpc_call"
                           data-method="action_do"
                           visible-only-if="fields.step == 'todo'"
                           class="button hollow ">Do</button>
                   <button data-function="rpc_call"
                           data-method="action_cancel"
                           visible-only-if="fields.step == 'todo'"
                           class="button hollow alert">Cancel</button>
               </div>
           </div>
           <div class="row">
               <div class="columns small-6 medium-6 large-6">
                   <fieldset>
                       <legend>Options</legend>
                       <label for="step" />
                       <field name="step" readonly="1"/>
                       <label for="priority"/>
                       <field name="priority"/>
                       <label for="complexity"/>
                       <field name="complexity"/>
                   </fieldset>
               </div>
               <div class="columns small-6 medium-6 large-6">
                   <fieldset>
                       <legend>Dates</legend>
                       <label for="creation_date"/>
                       <field name="creation_date" readonly="1"/>
                       <label for="deadline_date"/>
                       <field name="deadline_date"/>
                       <label for="ending_date"/>
                       <field name="ending_date" readonly="1"/>
                       <label for="cancelled_date"/>
                       <field name="cancelled_date" readonly="1"/>
                   </fieldset>
               </div>
           </div>
           <div class="row">
               <div class="columns">
                   <label for="description"/>
                   <field name="description" type="Html"/>
               </div>
           </div>
       </div>
    </template>
    <template id="AnyBlokTodoList">
       <field name="title" />
       <field name="step" />
       <field name="priority" />
       <field name="complexity" />
    </template>
</templates>
```

Maintenant nous allons indiquer dans l'espace fonctionnel les vues a utiliser

dans le fichier:
> anyblok_todolist/erpblok_todolist/space.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<records>
    <record external_id="setting_space_todolist">
        ...
        <field name="default_action" external_id="action_setting_todolist">
            ...
            <field name="views">
                <record external_id="view_todo_list">
                    <field name="selectable">1</field>
                    <field name="mode">Model.UI.View.List</field>
                    <field name="template">AnyBlokTodoList</field>
                </record>
                <record external_id="view_todo_form">
                    <field name="mode">Model.UI.View.Form</field>
                    <field name="template">AnyBlokTodoForm</field>
                </record>
            </field>
            <field name="transitions">
                <record external_id="transition_todo_select_record">
                    <field name="name">selectRecord</field>
                    <field name="mode">Model.UI.View.List</field>
                    <field name="code">open_view</field>
                    <field name="view" external_id="view_todo_form"/>
                </record>
                <record external_id="transition_todo_new_record">
                    <field name="name">newRecord</field>
                    <field name="mode">Model.UI.View.List</field>
                    <field name="code">open_view</field>
                    <field name="view" external_id="view_todo_form"/>
                </record>
            </field>
        </field>
    </record>
</records>
```

La vue formulaire a ajouté deux boutons, ces boutons font des appel au serveur, ils appellent les méthodes:
* action_do
* action_cancel

nous allons commencer par ajouter le fichier:
> anyblok_todolist/erpblok_todolist/todo.py

Dans ce fichier nous allons surcharger un modèle existant, Cela ne va pas écraser le modèle mais l'enrichir.

```python
from anyblok import Declarations


@Declarations.register(Declarations.Model)
class Todo:

    def action_do(self):
        if self.step == 'todo':
            self.do()

        return {'action': 'refresh', 'primary_keys': self.to_primary_keys()}

    def action_cancel(self):
        if self.step == 'todo':
            self.cancel()

        return {'action': 'refresh', 'primary_keys': self.to_primary_keys()}
```

Il ne reste plus qu'a definir l'import dans le blok
> anyblok_todolist/erpblok_todolist/__init__.py

```python
...

class BlokTodoListERPBlok(Blok):

    @classmethod
    def import_declaration_module(cls):
        from . import todo  # noqa

    @classmethod
    def reload_declaration_module(cls, reload):
        from . import todo
        reload(todo)
```

## Ajouter des bouttons en vue liste pour changer l'état

Nous allons denouveau modifier le modele **Todo** afin d'y ajouter des methodes de class:
* multi_action_do
* multi_action_cancel

Si une méthode d'instance agis sur une intance de todo, les méthodes de class agissent sur toutes les instances.

Modifiez le fichier:
> anyblok_todolist/erpblok_todolist/todo.py

```python
...

@Declarations.register(Declarations.Model)
class Todo:

    ...

    @classmethod
    def multi_action_do(cls, primary_keys, *args, **kwargs):
        for pks in primary_keys:
            self = cls.from_primary_keys(**pks)
            self.do()

        return {'action': 'refresh'}

    @classmethod
    def multi_action_cancel(cls, primary_keys, *args, **kwargs):
        for pks in primary_keys:
            self = cls.from_primary_keys(**pks)
            self.cancel()

       return {'action': 'refresh'}
```

Modifier l'espace fonctionnel
> anyblok_todolist/erpblok_todolist/space.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<records>
    <record external_id="setting_space_todolist">
        ...
        <field name="default_action" external_id="action_setting_todolist">
            ...
            <field name="buttons">
                <record external_id="buttons_todo_do_list">
                    <field name="label">Do</field>
                    <field name="mode">Model.UI.View.List</field>
                    <field name="function">rpc_call_classmethod</field>
                    <field name="method">multi_action_do</field>
                    <field name="on_readonly">1</field>
                    <field name="on_selected">1</field>
                </record>
                <record external_id="buttons_todo_cancel_list">
                    <field name="label">Cancel</field>
                    <field name="mode">Model.UI.View.List</field>
                    <field name="function">rpc_call_classmethod</field>
                    <field name="method">multi_action_cancel</field>
                    <field name="on_readonly">1</field>
                    <field name="on_selected">1</field>
                </record> 
            </field>
        </field>
    </record>
</records>
```

## Ajout d'une vue custom.

Pour ERPBlok nous nous sommes servis de la génération des vues fournit par le framework. Cependant dans certain cas nous souhaitons utiliser un affichage plus souple et plus personnalisé. Nous allons donc utiliser le client existant et le changer via ERPBlok.

### Modifier la définition du bloc.

Dans le fichier
> anyblok_todolist/erpblok_todolist/__init__.py

Nous allons pointer vers le client existant afin que celui ci soit chargé

```python
...

class BlokTodoListERPBlok(Blok):

    ...

    client_js_babel = [
        '/todolist-client/client/todolist.jsx',
    ]

    ...
```

### Ajouter un nouvel espace fonctionnel.

Bien que les deux espaces traite la même fonctionnalité, la séparation en deux espaces est purement d'ordre pratique et visuel. En effet il aurait fallut ajouter un menu dans le premier espace. De plus la séparation permet de bien montrerr les différence d'interface.

Dans le fichier:
> anyblok_todolist/erpblok_todolist/space.xml

Nous allons ajouter le nouvel espace

```xml
...
<records>
    ...
    <record external_id="setting_space_todolist_custom">
        <field name="label">Todolist App</field>
        <field name="icon">fi-database</field>
        <field name="description">ERPblok custom view for todolist</field>
        <field name="category" external_id="todolist_space_category" />
        <field name="default_action" external_id="action_setting_todolist_app">
            <field name="model">Model.Todo</field>
            <field name="label">Todolist</field>
            <field name="views">
                <record external_id="view_todo_custom">
                    <field name="selectable">1</field>
                    <field name="mode">Model.UI.View.Custom</field>
                    <field name="template">AnyBlokTodoCustom</field>
                </record>
            </field>
        </field>
    </record>
</records>
```

### Ajouter un template de vue

Ici le template est très simple il ne doit servir que de point d'entrée au composant **Reactjs**. Il faut donc ajouter un div et un script **javascript** pour charger le client.

Dans le fichier
> anyblok_todolist/erpblok_todolist/todolist.tmpl

```xml
<templates>
    ...
    <template id="AnyBlokTodoCustom">
        <div id="erpblok-todolist-app"></div>
        <script >
            var app = ReactDOM.render(
                React.createElement(TodoListApp, {}),
                document.getElementById('erpblok-todolist-app'));
            app.load_initial_entries();
        </script>
    </template>
</templates>
```

Mettez votre serveur a jour. Le petit client est désormais disponible a l'interrieur d'ERPBlok.
