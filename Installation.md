## From the shinken.IO

The easiest solution to set up the Shinken WebUI is to use the CLI *shinken install* which will install from [Shinken.IO](http://shinken.io/).

* Install the WebUI
```
# Log in with your shinken user account ...
$ su - shinken

# Install package ...
$ shinken install webui2
```

> Note that the shinken.io package is **webui2**! This to avoid conflicting with a previous installation of the WebUI.

* Install python dependencies

Then you also need to install some python dependencies using pip (depending on your distribution, you could also install theses packages from your distribution repositories):

```
$ sudo pip install pymongo>=3.0.3 requests arrow bottle==0.12.8
```

> The packages required are listed in the `requirements.txt` file.

* Install mongodb

By the time, `mongodb` is also mandatory. If you did not yet installed `mongodb`, please install it: 
```
$ sudo apt-get install mongodb
```

* And declare it into the modules of the broker configuration :
```
$ cat /etc/shinken/brokers/broker-master.cfg
[...]
modules     webui2
[...]
```

* Restart Shinken
```
$ sudo service shinken restart
```

The configuration file (`webui2.cfg`) is located in the etc/shinken/modules directory and is self explanatory. You can look at the [configuration documentation](configuring) for more informations.

Despite the WebUI is fully operational out of the box, you may want to look how to enable modules like [authentication](ins-authenticating), [preferences](ins-storing), [graphs](ins-graphing) and even [logs & availability](ins-logs).

## Updating from previous installation

**Note:** do not forget to save your `webui2.cfg` configuration file before installing the new version!

```
# Log in with your shinken user account ...
$ su - shinken

# Save previous configuration ...
$ cp /etc/shinken/modules/webui2.cfg /etc/shinken/modules/webui2.bak

# Install update package ...
$ shinken install webui2

# Restore previous configuration ...
$ cp /etc/shinken/modules/webui2.bak /etc/shinken/modules/webui2.cfg
```

## Expert install: from the Github repository

Assuming you already installed from Shinken.IO, you simply need to replace the content of your *modules/webui* directory with the content of the *module* directory from the github repository.
```
[Get a release from the project repo]
$ wget https://github.com/shinken-monitoring/mod-webui/archive/branch.tar.gz
$ tar -xvf branch.tar.gz

[Stop Shinken]
$ sudo service shinken stop

[Update application]
$ sudo rm -R /var/lib/shinken/modules/webui2/.
$ sudo cp -R mod-webui-branch/* /var/lib/shinken/modules/webui2/.

[Start Shinken]
$ sudo service shinken start
```

Your configuration file is located in the *etc/modules/webui2.cfg* and it will not be affected by the previous copy.

> Your installation directories (etc, modules) are located in the *.shinken.ini* file of your home directory.