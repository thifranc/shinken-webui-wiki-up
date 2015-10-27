
The WebUI can use external modules to display logs and availability stats corresponding to the services and hosts.

By default, a mongodb module is embedded in the WebUI (you don't need to install it). To use it, you just need to install mongodb (with your distribution packages) and pymongo (with pip or with your distribution packages). It's very easy and doesn't need any configuration.

> Of course, you cannot use many logs modules. If more than one are installed, the first one will be used.

## Mongo-logs

This module is embedded in the WebUI. You **must not declare** this module in the `webui2.cfg` file or it will break things in the WebUI (such as login ...).

You only need to install mongodb:

On Debian/Ubuntu
```
$ aptitude install mongodb pymongo
```

On Redhat/Centos:
**TODO**

### Configuration

Please note that the default parameters do not need to be modified.

```
   # Mongodb parameters for internal Web UI modules
   # NOTE: Do not change these parameters unless you are using the 'mongo-logs' module
   # with different parameters than the default ones.

   # Database URI
   #uri                        mongodb://localhost

   # If you are running a MongoDB cluster (called a “replica set” in MongoDB),
   # you need to specify it's name here.
   # With this option set, you can also write the mongodb_uri as a comma-separated
   # list of host:port items. (But one is enough, it will be used as a “seed”)
   #replica_set

   # Database name where to fetch the logs/availability collections
   #database                   shinken
   # User authentication for database access
   #username
   #password

   # Logs collection name
   #logs_collection            logs

   # Hosts availability collection name
   #hav_collection             availability
```