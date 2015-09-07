# Design and development


This file needs to be completed and updated !!!


## WebUI integration with Shinken

The WebUI is an external broker module. As of it, it's an external process launched by the main broker. It consumes broks (broker messages) to update its *DataManager* thanks to a *Regenerator*. It can get *all* Shinken broks if you place this broker in your top level realm.

The WebUI is also *almost* a daemon because it loads its own modules as a broker. The modules loaded by the WebUI allow to enrich its features with user authentication, preferences storage, graphing metrics, ...

The WebUI does not use a database to get data. Instead, it uses a "Regenerator" object that will consume each brok, and will "recreate" all Hosts/Services/Contacts objects as if the code was in the scheduler daemon. So basically, WebUI and pages always reflect the **current state** of monitoring objects.

The WebUI HTTP layer is handled with bottle.py (http://bottlepy.org/docs/dev/index.html). Bottle is a fast, simple and lightweight WSGI micro web-framework for Python. It will use the best backend it finds. We recommend using python-cherrypy which is multi-threaded (default SWSGI is single-threaded), so please use it if you want to avoid locking problems in a production environment ...


### How does the WebUI function globally ? 

There are 2 threads running concurrently:
  * the main thread is the bottle.py (Http) listener. 
  * the 2nd thread is the thread that gets broks and gives them to the regenerator object to update the data model.

There is a lock system between the threads. You can have several Http requests in progress without problem, but during this time the renererator will wait. During execution of the regenerator, no requests are processed. This is not considered as a problem, as the regenerator is very fast, and there is only a 1s lock for a 7k service conf loading, so it's not a huge problem.


### Plugins loading

On start, the Web UI tries to load:
- the additional plugins (if some exist ...) located in the additional plugins directory (*additional_plugins_dir*).
- the additional plugins (if some exist ...) provided by the Web UI modules that declare a function named `get_webui_plugins_path`
- the embedded plugins located in the *modules/webui/plugins* directory

Loading a plugin is:
- declaring views (templates)
- declaring routes(relations between URI and Python functions)
- declaring widgets
- runnning the *load_config* function declared in the plugin if it exists ... this function allows the plugins to load configuration parameters before starting to be used.

If a plugin cannot be loaded correctly, the Web UI logs an ERROR message.

Once it loaded its plugins the Web UI starts the Web server.

### What is a WebUI User? 

A WebUI user is a contact and you must have a contact named as the user that authenticates. The WebUI uses authentication modules for enabling user access (see installation doc).

[conf-users]: 


https://github.com/shinken-monitoring/mod-webui/wiki/Configuration---WebUI-Users


### To create a new page? 

All pages are in the *modules/webui/plugins* directory. Each "plugin" has its own directory. A plugin (like the eltdetail, element detail) can have more than one "page" (like /host or /service for eltdetail).

From here we will suppose you want to create a "/contact" page. You can copy the dummy example in the plugins directory into a contact directory.

You need to change the *contact/dummy.py* file into a *contact/contact.py* file, because the WebUI will search for a *dir/dir.py* file to "load".

### Hooks

 The **load_config** function is called on plugin loading. It allows for the plugin to define specific parameter ...

 To be developed ...

### Routes

 To be developed ...

### Templates 

Templates are HTML with some Python code in it in the *plugins/XxX/views* directory for specific templates, and the "global layout" (menu and all common part) in the *webui/views* directory.

Your template must call the template layout (layout.tpl in the *webui/views* directory) with a "rebase" call.

If you put the rebase call at the beginning of your template, all which is below will be placed "inside" the layout that will call the common javascripts and css, make the menu and then call your html/python code.

The rebase call can take arguments:
  * title: title of the page
  * js and css list: list of YOUR specific javascript or css to call AFTER the common ones.
  * refresh: boolean, set the refresh to configured value or do not refresh at all

Example of a call, taken from a view:
```
  %rebase("layout", css=['groups/css/groups-overview.css'], js=['groups/js/groups-overview.js'], title='Hosts groups overview', refresh=True)
```


### Call Python objects in your templates 

More information are available  on the [Bottle template engine page](http://bottlepy.org/docs/dev/stpl.html).

Templates are mainly HTML code but you can call python code inside them. The dict you returns in the *plugin.py* call will be available in this page.

For example, the my_host variable will be available in the page with the host object you find (or None if your give it a bad name of course). Then you can "call" python with for example here to show the host_name: ``<span>My host name is {{my_host.host_name}} </span>`` is a way in the templates to "call a variable or function" that will return a string, here the host_name. 

If the function or property you call must return HTML code that SHOULD be output as it, without "protection", you must put a ! before your call.

Let use an example where the output value of an host is in fact an html output. If you use the code  ``my_host.output``, bottle.py will "protect" the html generated by escaping the string, and change <> characters with escaped value. You can change this by calling ``my_host.output``. Of course, it's a security problem if the output is with a javascript code...

You can also "loop" or put "if" in your template. You just need to put your code with a % before. Like:

```
  %for i in xrange(1, 10):
    My valus is  {{i}}
  %end
```

You must put the %end here, the indentation is NOT enough ...

For an if it's also easy:

```
  %if host.active_checks_enabled:
    The host got active checks enabled
  %else:
    The host DO NOT have active check enabled
  %end
```