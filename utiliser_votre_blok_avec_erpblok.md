# Utiliser votre Blok avec ERPBlok

                                                                               
1) Use todolist in ERPBlok                                                      
                                                                                
Install ERPBlok                                                                 
                                                                                
hg clone http://bitbucket.org/jssuzanne/erpblok erpblok                         
cd erpblok                                                                      
../sandbox/bin/python setup.py install                                          
cd ..                                                                           
sandbox/bin/anyblok_pyramid --db-name todolist --db-driver-name postgresql      
                                                                                
in a browser                                                                    
                                                                                
localhost:5000/                                                                 
                                                                                
2) Add the new blok ``erpblok-todolist``                                        
                                                                                
2.1) add the dependency anyblok_pyramid in the setup.py of the distribution    
                                                                                
::                                                                              
                                                                                
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
                'todolist-rest-api=anyblok_todolist.todolist:BlokTodoList',     
                'todolist-client=anyblok_todolist.client:BlokTodoListClient',   
                'erpblok-todolist=anyblok_todolist.erpblok_todolist:BlokTodoListERPBlok',
            ],                                                                  
        },                                                                      
    )                        
    
 
                                                                                
2.2) Add a new Blok, which inherit of todolist                                 
                                                                                
.. anyblok_todolist                                                             
.. ...                                                                          
.. .. todolist                                                                  
.. .. ...                                                                       
.. .. client                                                                    
.. .. ...                                                                       
.. .. erpblok_todolist                                                          
.. .. .. __init__.py                                                            
                                                                                
::                                                                              
                                                                                
   from anyblok.blok import Blok                                                
                                                                                
                                                                                
   class BlokTodoListERPBlok(Blok):                                             
       version = '0.1.0'                                                        
                                                                                
       required = ['todolist', 'erpblok-core']                                  
                                                                                
2.3) Update the distribution                                                    
                                                                                
cd anyblok_todolist                                                             
../sandbox/bin/python setup.py develop                                          
cd ..                                                                           
                                                                                
2.4) Install the blok                                                           
                                                                                
sandbox/bin/anyblok_updatedb --db-name todolist --db-driver-name postgresql --install-bloks erpblok-todolist
                                                                                
2.5) start the server (only for test, for production use gunicorn)             
                                                                                
sandbox/bin/anyblok_pyramid --db-name todolist --db-driver-name postgresql      
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
  
                                                                               
3) add Space                                                                    
                                                                                
space category ::                                                               
                                                                                
    <?xml version="1.0" encoding="UTF-8"?>                                      
    <records>                                                                   
        <record external_id="todolist_space_category">                          
            <field name="label">Todolist</field>                                
        </record>                                                               
    </records>                                                                  
                                                                                
add space ::                                                                    
                                                                                
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
                                                                                
add data in blok ::                                                             
                                                                                
    ...                                                                         
                                                                                
    class BlokTodoListERPBlok(Blok):                                            
        ...                                                                     
                                                                                
        def update(self, latest_version):                                       
            super(BlokTodoListERPBlok, self).update(latest_version)             
            self.import_file('xml', 'Model.Web.Space.Category',                 
                             'spaces.category.xml')                             
            self.import_file('xml', 'Model.Web.Space', 'space.xml')             
                                                                                
update blok                                                                     
                                                                                
sandbox/bin/anyblok_updatedb --db-name todolist --db-driver-name postgresql --update-bloks erpblok-todolist

4) add view                                                                     
                                                                                
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    class BlokTodoListERPBlok(Blok):                                            
        ...                                                                     
                                                                                
        views = [                                                               
            'todolist.tmpl',                                                    
        ]                                                                       
 
 ::                                                                              
                                                                                
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

                                                                                
::                                                                              
                                                                                
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
    
                                                                                  
::                                                                              
                                                                                
    ...                                                                         
                                                                                
    class BlokTodoListERPBlok(Blok):                                            
                                                                                
        @classmethod                                                            
        def import_declaration_module(cls):                                     
            from . import todo  # noqa                                          
                                                                                
        @classmethod                                                            
        def reload_declaration_module(cls, reload):                             
            from . import todo                                                  
            reload(todo)                                                        
           
                                                                                
::                                                                              
                                                                                
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
 
 
                                                                                
5) Do and cancel on list view                                                   
                                                                                
::                                                                              
                                                                                
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
                                                                                
::                                                                              
                                                                                
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
                                                                                
                                                                                
6) Add a custom view                                                            
                                                                                
::                                                                              
                                                                                
    client_js_babel = [                                                         
        '/todolist-client/client/todolist.jsx',                                 
    ]                                                                           
                                                                                
                                                                                
::                                                                              
                                                                                
    <?xml version="1.0" encoding="UTF-8"?>                                      
    <records>                                                                   
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
                                                                                
                                                                                
::                                                                              
                                                                                
    <templates>                                                                 
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
