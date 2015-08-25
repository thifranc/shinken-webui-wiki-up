The WebUI can use external modules to store common and user's preferences: dashboard, default parameters, bookmarks, etc.

By default, a mongodb module is embedded in the WebUI (you don't need to install it). To use it, you just need to install mongodb (with your distribution packages) and pymongo (with pip or with your distribution packages). It's very easy and doesn't need any configuration.

> Of course, you cannot use many preferences modules. If more than one are installed, the first one will be use.

## Mongodb

This module is now embedded in the WebUI. You only need to install mongodb:

On Debian/Ubuntu
```
$ aptitude install mongodb pymongo
```

On Redhat/Centos:
**TODO**

### Configuration

Default parameters do not need to be modified.

How to configure:
```
$ cat /etc/shinken/modules/mongodb.cfg
[...]

## Module:      Mongodb
## Loaded by:   Arbiter, WebUI
# In Arbiter: Read objects in a mongodb database (like hosts or services).
# In WebUI: Save/read user preferences.
define module {
    module_name     mongodb
    module_type     mongodb
    uri             mongodb://localhost/?safe=false
    database        shinken
    #username        username     ;optional
    #password        password     ;optional

    #replica_set                  ;Advanced option if you are running a cluster environnement
}

## SQLite

> The sqlite module is not documented, has not been tested and should not be used except if you know exactly what you are doing. Please notice no support will be provided if you use this module. Some information may be found in the Shinken documentation if needed. 
