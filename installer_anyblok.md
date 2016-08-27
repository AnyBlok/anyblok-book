# Installer AnyBlok 


## Créer un virtualenv

                                                         
                                                                                
pyvenv-3.5 sandbox                                                              
                                                                                
2) installer anyblok                                                            
                                                                                
sandbox/bin/pip install anyblok                                                 
                                                                                
3) Installer une BDD (postgres + psycopg2)                                      
                                                                                
sandbox/bin/pip install psycopg2                                                
                                                                                
3) Créer une base                                                               
                                                                                
sandbox/bin/anyblok_createdb --db-name todolist --db-driver-name postgresql     
==> Load config file '/Library/Application Support/AnyBlok/conf.cfg'            
==> Load config file '/Users/jssuzanne/Library/Application Support/AnyBlok/conf.cfg'
                                                                                
4) Lancer un interpreter                                                        
                                                                                
sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql  
==> Load config file '/Library/Application Support/AnyBlok/conf.cfg'            
==> Load config file '/Users/jssuzanne/Library/Application Support/AnyBlok/conf.cfg'
==> >>>                                                                         
                                                                                
Congrate voici la première base AnyBlok d'installé                              
                                                                                
5) lister les bloks disponible                                                  
                                                                                
>>> registry.System.Blok.query().all()                                          
>>> [anyblok-core (installed), anyblok-io (uninstalled), anyblok-io-csv (uninstalled), anyblok-io-xml (uninstalled), model_authz (uninstalled)]
                                                                                
5 bis) pour les amoureux de IPython                                             
                                                                                
sandbox/bin/pip install ipython                                                 
sandbox/bin/anyblok_interpreter --db-name todolist --db-driver-name postgresql  
==> Load config file '/Library/Application Support/AnyBlok/conf.cfg'            
==> Load config file '/Users/jssuzanne/Library/Application Support/AnyBlok/conf.cfg'
==> In [1]: registry.System.Blok.query().all()                                  
==> Out[1]:                                                                     
==> [anyblok-core (installed),                                                  
==>  anyblok-io (uninstalled),                                                  
==>  anyblok-io-csv (uninstalled),                                              
==>  anyblok-io-xml (uninstalled),                                              
==>  model_authz (uninstalled)]  
