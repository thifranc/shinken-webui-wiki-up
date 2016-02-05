The WebUI can use external modules to store common and user's preferences: dashboard, default parameters, bookmarks, etc.

By default, a mongodb module is embedded in the WebUI (you don't need to install it). To use it, you just need to install mongodb (with your distribution packages) and pymongo (with pip or with your distribution packages). It's very easy and doesn't need any configuration.

> Of course, you cannot use many preferences modules. If more than one are installed, the first one will be use.

## Mongodb

This module is now embedded in the WebUI. You only need to install MongoDB. 

*More information [here](https://docs.mongodb.org/getting-started/shell/tutorial/install-on-linux/) for installing MongoDB Community edition on Linux distros.*


On Debian/Ubuntu
```
$ sudo apt-get install mongodb-org
```

On Redhat/Centos:
```
$ sudo yum install mongodb-org
```

### Configuration

Please note that default parameters do not need to be modified.

```
uri             mongodb://localhost/?safe=false
database        shinken
```

## SQLite

> The sqlite module is not documented, has not been tested and should not be used except if you know exactly what you are doing. Please notice that no support will be provided if you use this module. Some information may be found in the Shinken documentation if needed. 