# Installing the Web UI

Shinken can provide a self sufficient Web User Interface, which includes its own web server (No need to setup Apache or Microsoft IIS)

This Shinken WebUI is started at the same time as the Shinken broker. It is configured by setting a few basic parameters in several modules.

WebUI is built upon a main application using several modules:
- authentication modules, used to authenticate users that log in
- storing modules, used to make parameters persistent
- graphing modules, used to display graphs built from metrics
- … (helpdesk, logs…)

**TO BE COMPLETED**:
```
- propose a set of "standard" modules for which WebUI consistency is maintained
- explain the WebUI and modules interaction ... especially configuration consistency!
```

## Installing the main application

The main application is easily installed with the Shinken CLI.
```
$ shinken install webui
```

Then you also need to install some python dependencies using pip (depending on your distribution, you could also install theses packages from your distribution repositories):
```
$ pip install pymongo requests arrow
```

The detailed installation procedure is available [on this page](https://github.com/shinken-monitoring/mod-webui/wiki/Installing-Shinken-WebUI).

The configuration file (*webui.cfg*) is located in the *etc/shinken/modules* directory and is self explanatory.

**TO BE COMPLETED**:
```
- installing from Github repo
- how-to updating
- python requirements
```


## Installing authentication modules

The WebUI uses external modules to lookup your user password and allow/deny access to the interface.

By default it is using the auth-cfg-password module, which will look into your contact definition for the password parameter. 

More information about authentication modules and their installation/configuration [on this page](https://github.com/shinken-monitoring/mod-webui/wiki/Installing-WebUI-authentication-modules).

## Installing user's preferences modules

The WebUI is self sufficient to store common and user's preferences: dashboard, default parameters, ... It uses files to store the user's preferences. It is whenever possible to store user preferences in a MongoDB or Sqlite database.

More information about storage modules and their installation/configuration [on this page](https://github.com/shinken-monitoring/mod-webui/wiki/Installing-WebUI-storage-modules).

## Metrology graph modules

You can link the WebUI so it will contextually display graphs from other tools, like PNP4Nagios or Graphite. All you need is to install and configure a graphing module.

More information about graphing modules and their installation/configuration [on this page](https://github.com/shinken-monitoring/mod-webui/wiki/Installing-WebUI-graph-modules).