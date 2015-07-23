It exists two storage modules that are to be used with the Shinken WebUI: mongodb and Sqlite. We recommand using the mongodb module because it is the one that is best integrated and tested with the WebUI.

> The sqlite module is not documented, some information may be found in the Shinken documentation if needed.

## Mongodb
The mongodb module is the one which is best integrated with the WebUI. Default parameters do not need to be modified.

How to install:
```
$ shinken install mongodb
```

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

$ cat /etc/shinken/modules/webui.cfg
[...]
modules     auth-cfg-password, mongodb
[...]
```


