# Créer son premier Blok

                                                                                
1) Créer un paquet python                                                       
                                                                                
lire le tutaux sur les paquet python pour le pypi                               
http://sametmax.com/creer-un-setup-py-et-mettre-sa-bibliotheque-python-en-ligne-sur-pypi/
                                                                                
Structure du package                                                            
                                                                                
anyblok_todolist                                                                
.. anyblok_todolist                                                             
.. .. __init__.py                                                               
.. .. todolist                                                                  
.. .. .. __init__.py                                                            
.. setup.py                                                                     
                                                                                
1.1) Fichier setup                                                              
                                                                                
::                                                                              
                                                                                
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
                                                                                   
1.2) Fichier de description du blok ``anyblok_todolist/anyblok_todolist/todolist/__init__.py``
                                                                                
::                                                                              
                                                                                
    from anyblok.blok import Blok                                               
                                                                                
                                                                                
    class BlokTodoList(Blok):                                                   
        version = '0.1.0'                                                       
                                                                                
                                                                                
2) Installer le nouveau package.                                                
                                                                                
cd anyblok_todolist                                                             
../sandbox/bin/python setup.py develop                                          
cd ..                                                                           
                                                                                
3) Vérifier que le nouveau blok existe.                                         
                                                                                
sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
==> In [1]: registry.System.Blok.query().all()                                  
==> Out[1]:                                                                     
==> [anyblok-core (installed),                                                  
==>  anyblok-io (uninstalled),                                                  
==>  anyblok-io-csv (uninstalled),                                              
==>  anyblok-io-xml (uninstalled),                                              
==>  model_authz (uninstalled),                                                 
==>  todolist (uninstalled)]                                                    
                                                                                
4) Installer le blok todolist                                                   
                                                                                
sandbox/bin/anyblok_updatedb --db-name todolist --db-driver-name postgresql --install-bloks todolist
sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql     
==> In [1]: registry.System.Blok.query().all()                                  
==> Out[1]:                                                                     
==> [anyblok-core (installed),                                                  
==>  anyblok-io (uninstalled),                                                  
==>  anyblok-io-csv (uninstalled),                                              
==>  anyblok-io-xml (uninstalled),                                              
==>  model_authz (uninstalled),                                                 
==>  todolist (installed)]       

                                                                                
5) Lister les ``Model`` existant.                                               
                                                                                
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
==>  'Model.Documentation']         