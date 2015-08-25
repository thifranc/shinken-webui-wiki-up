# Installing the Web UI

Shinken can provide a self sufficient Web User Interface, which includes its own web server (No need to setup Apache or Microsoft IIS)

This Shinken WebUI is started at the same time as the Shinken broker. It is configured by setting a few basic parameters in several modules.

WebUI is built upon a main application and can use several modules to delegate some actions:
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

The configuration file (`webui.cfg`) is located in the `etc/shinken/modules` directory and is self explanatory.


## Upgrading the application

**TO BE COMPLETED **


# Modules

## Authentication modules

The WebUI can use external modules to check user/password before allowing access to the interface.

By default, the`auth-cfg-password` module is embedded in the WebUI (you don't need to install it). I will look for the `password` parameter into your contact definitions (contacts.cfg, for instance).

Important note: you can use as many authentication modules as you want. For instance, you can combine the default config file authentication with an active directory.

More information about authentication modules and their installation/configuration [on this page](https://github.com/shinken-monitoring/mod-webui/wiki/Installing-WebUI-authentication-modules).

## User's preferences modules

The WebUI can use external modules to store common and user's preferences: dashboard, default parameters, bookmarks, etc.

By default, a `mongodb` module is embedded in the WebUI (you don't need to install it). To use it, you just need to install mongodb (with your distribution packages) and pymongo (with pip or with your distribution packages). It's very easy and doesn't need any configuration.

Of course, you cannot use many preferences modules. If more than one are installed, the first one will be use.

More information about storage modules and their installation/configuration [on this page](https://github.com/shinken-monitoring/mod-webui/wiki/Installing-WebUI-storage-modules).

## Metrology graph modules

The WebUI can use external modules to display graphs corresponding to the services directly in the WebUI.

Currently there is two graphs systems supported : PNP4Nagios and Graphite.

Note that the WebUI do not include graph's links directly, but will proxy the images. So you can configure PNP4Nagios and Graphite to listen only on localhost (or a limited set of IPs), and count on the WebUI to secure the access to these graphs (showing them only to authenticated and concerned users).

More information about graphing modules and their installation/configuration [on this page](https://github.com/shinken-monitoring/mod-webui/wiki/Installing-WebUI-graph-modules).