# Installing the Web UI

Shinken includes a self sufficient Web User Interface, which includes its own web server (No need to setup Apache or Microsoft IIS)

Shinken WebUI is started at the same time Shinken itself does, and is configured by setting a few basic parameters in several modules.

WebUI is built upon a main application using several modules:
- authentication modules, used to authenticate users that log in
- storing modules, used to make parameters persistent
- graphing modules, used to display graphs built from metrics

## Installing the main application

The main application is easily installed with the Shinken CLI.
```
$ shinken install webui
```

The configuration file (*webui.cfg*) is located in the *etc/shinken/modules* directory and is self explanatory.

More details for installing and configuration [on this page](https://github.com/mohierf/mod-webui/wiki/Installing-and-configuring-Shinken-WebUI).

## Installing authentication modules

The WebUI uses external modules to lookup your user password and allow/deny access to the interface.

By default it is using the auth-cfg-password module, which will look into your contact definition for the password parameter. 

More information about authentication modules and their installation/configuration [on this page](https://github.com/mohierf/mod-webui/wiki/Installing-WebUI-authentication-modules).

## Installing user's preferences modules

The WebUI is self sufficient to store common and user's preferences: dashboard, default parameters, ... It uses files to store the user's preferences. It is whenever possible to store user preferences in a MongoDB or Sqlite database.

More information about storage modules and their installation/configuration [on this page](https://github.com/mohierf/mod-webui/wiki/Installing-WebUI-storage-modules).

## Metrology graph modules

You can link the WebUI so it will contextually display graphs from other tools, like PNP4Nagios or Graphite. All you need is to install and configure a graphing module.

More information about graphing modules and their installation/configuration [on this page](https://github.com/mohierf/mod-webui/wiki/Installing-WebUI-graph-modules).