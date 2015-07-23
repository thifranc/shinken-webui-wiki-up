## From the shinken.IO

The easiest solution to set up the Shinken WebUI is to use the CLI *shinken install* which will install from Shinken.IO.

* Install the WebUI
```
$ shinken install webui
```

* And declare it into the modules of the broker configuration :
```
$ cat /etc/shinken/brokers/broker-master.cfg
[...]
modules     webui
[...]
```

* Install an authentication module. For instance:
```
$ shinken install auth-cfg-password
```

* Add it into the modules of the WebUI:
```
$ cat /etc/shinken/modules/webui.cfg
[...]
modules     auth-cfg-password
[...]
```

* Restart Shinken
```
$ sudo service shinken restart
```

## From the Github repository

Assuming you already installed from the shinken.IO, you simply need to replace the content of your *modules/webui* directory with the content of the *module* directory from the github repository.
```
[Get a release from the project repo]
$ wget https://github.com/shinken-monitoring/mod-webui/archive/BS3-1.0.tar.gz
$ tar -xvf BS3-1.0.tar.gz

[Stop Shinken]
$ sudo service shinken stop

[Update application]
$ cp -R mod-webui-BS3-1.0/* /var/lib/shinken/modules/webui/.

[Start Shinken]
$ sudo service shinken start
```

Your configuration file is located in the *etc/modules/webui.cfg* and it will not be affected by the previous copy.

> Your installation directories (etc, modules) are located in the *.shinken.ini* file of your home directory.

