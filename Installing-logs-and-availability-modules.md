
The WebUI can use external modules to display logs and availability stats corresponding to the services and hosts.

By default, a mongodb module is embedded in the WebUI (you don't need to install it). To use it, you just need to install mongodb (with your distribution packages) and pymongo (with pip or with your distribution packages). It's very easy and doesn't need any configuration.

> Of course, you cannot use many logs modules. If more than one are installed, the first one will be use.

## Mongo-logs

**TODO** : Document the installation of the broker module.

This module is embedded in the WebUI. You only need to install mongodb:

On Debian/Ubuntu
```
$ aptitude install mongodb pymongo
```

On Redhat/Centos:
**TODO**

### Configuration

Please note that default parameters do not need to be modified.

```
uri             mongodb://localhost/?safe=false
database        shinken
```