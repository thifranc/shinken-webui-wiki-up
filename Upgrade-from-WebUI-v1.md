Upgrading from WebUI version 1 is made really easy. 

* Proceed to the three first steps of the new Web UI installation as explained [on this page](https://github.com/shinken-monitoring/mod-webui/wiki/Installation) : 

  - install the new WebUI module
  - install python dependencies
  - install mongodb (not mandatory but recommended) 

** With the current version 2.0, do not change anything in your broker configuration file! Some side effects may exist with some Shinken daemons :/P It also may be mandatory to remove the old `/var/lib/shinken/modules/webui` directory to avoid confusing the main broker ...
* Edit your broker configuration file and replace `webui` with `webui2` in the modules list:
```
$ vi /etc/shinken/brokers/broker-master.cfg
[...]
modules     ..., webui2, ...
[...]
```

* Edit the new Web UI configuration file to update the configuration:
```
$ vi /etc/shinken/modules/webui2.cfg
```

> This page will be helpful to set up the configuration: [Configuring Web UI](https://github.com/shinken-monitoring/mod-webui/wiki/Configuration---Main-parameters). 

* Restart Shinken
```
$ sudo service shinken restart
```

**Notes:**
* You do not need anymore to include `auth-cfg-password`, `auth-htpasswd` or `mongodb` in the Web UI modules list.