# Installer AnyBlok 

AnyBlok est un framework python. Il utilise l'ORM SQLAlchemy. Pour les besoin du tutorial nous allons utilisé Postgresql comme serveur de base de donnée. Ce choix est lié uniquement au fait que je maîtrise d'avantage cette BDD que les autres. Si vous souhaitez utilisé une autre BDD, vous devrez adapter les partis installation et l'option de commande --db-driver-name. 


## Préparer votre instance de travail.


Dans un premier temps installer votre serveur de base de donnée préféré.


> sudo apt-get install postgresql

Pour plus d'information, vous pouvez suivre les indications des documentations suivantes:
* [Ubuntu]([https://doc.ubuntu-fr.org/postgresql](https://doc.ubuntu-fr.org/postgresql)
* [Debian](https://wiki.debian.org/PostgreSql)
* [Fedora](http://doc.fedora-fr.org/wiki/Installation_et_configuration_de_PostgreSQL)
* [Mac OS avec macport](https://coderwall.com/p/xezzaa/install-postgresql-9-2-on-os-x-mountain-lion)

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
