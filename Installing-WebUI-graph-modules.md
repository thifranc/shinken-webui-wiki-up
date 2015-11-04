The WebUI can use external modules to display graphs corresponding to the services directly in the WebUI.

Currently there is two graphs systems supported : PNP4Nagios and Graphite.

> You can use as many graph modules as you want, but in most cases only one should be enough.

Note that the WebUI do not include graph's links directly, but will proxy the images. So you can configure PNP4Nagios and Graphite to listen only on localhost (or a limited set of IPs), and rely on the WebUI to secure the access to these graphs (showing them only to authenticated and concerned users).

## PNP graphs

**Note**: this integration has not been tested for a while. We are interested in any feedback about it! Thanks to report ...

You can ask for a PNP integration with the `ui-pnp` module. 

How to install:
```
$ shinken install ui-pnp
```

How to configure:
```
$ cat /etc/shinken/modules/ui-pnp.cfg
[...]

## Module:      ui-pnp
## Loaded by:   WebUI
# Use PNP graphs in the WebUI
define module {
    module_name     ui-pnp
    module_type     pnp_webui
    uri             http://YOURSERVERNAME/pnp4nagios/   ; Set your PNP4Nagios URI. Note : YOURSERVERNAME will be
                                                        ; changed by your broker hostname
}

$ cat /etc/shinken/modules/webui2.cfg
[...]
modules     ui-pnp
[...]
```

Shinken will automatically replace YOURSERVERNAME with the broker hostname at runtime to try and make it work for you, but you MUST change it to the appropriate value.


## Graphite graphs

You can view Graphite graphs with the `ui-graphite2` module.

**Note**: to feed your Graphite with metrics data you should have installed the `graphite2` module in Shinken. To proceed, see: https://github.com/shinken-monitoring/mod-graphite/tree/refactored

How to install:
```
$ shinken install ui-graphite2
```

How to configure:
```
$ cat /etc/shinken/modules/ui-graphite2.cfg
[...]
## Module:      ui-graphite2
## Loaded by:   WebUI
# Use Graphite graphs in the WebUI, based on default or graphite URL API templates.
#
# IMPORTANT : Set the proper TIME_ZONE parameter in graphite : webapp/graphite/local_settings.py
# Set if to match the system setting. If not, 4h graphs will be broken.
define module {
   module_name             ui-graphite
   module_type             graphite-webui2

   uri                     http://YOURSERVERNAME/
                           ; Set your Graphite URI. Note : YOURSERVERNAME will be
                           ; changed by your broker hostname

   # Specify the path where to search for template files
   templates_path          /var/lib/shinken/share/templates/graphite/

   # Optionally specify a source identifier for the metric data sent to Graphite.
   # This can help differentiate data from multiple sources for the same hosts.
   #
   # Result is:
   # host.GRAPHITE_DATA_SOURCE.service.metric
   # instead of:
   # host.service.metric
   #
   # Note: You must set the same value in this module and in the Graphite module configuration.
   #
   # default: the variable is unset
   #graphite_data_source shinken

   # Graph configuration for dashboard widget
   # Define font size and graph size for the dashboard widget
   dashboard_view_font     8
   dashboard_view_width    320
   dashboard_view_height   240

   # Graph configuration for element detail view
   # Define font size and graph size for the elment graphs
   detail_view_font        10
   detail_view_width       786
   detail_view_height      308

   # Optionnaly specify a service description for host check metrics
   #
   # Graphite stores host check metrics in the host directory whereas services
   # are stored in host.service directory. Host check metrics may be stored in their own
   # directory if it is specified.
   #
   # default: __HOST__
   #hostcheck               __HOST__

   # Optionnaly specify extra metrics
   # warning, critical, min and max information for the metrics are sometimes not necessary
   # in Graphite
   # You may specify which one are to be displayed or not
   # Default is to display all the information
   #use_warning             True
   #use_critical            True
   #use_min                 True
   #use_max                 True

   # Define colors to use for extra metrics
   # Default is black
   color_warning           orange
   color_critical          red
   color_min               black
   color_max               blue

   # Define some graphs parameters
   # Line mode
   # Possible values are : slope, staircase, connected
   # Default is connected
   #lineMode                connected

   # Graph time zone
   # Default is Europe/Paris
   #tz                      Europe/Paris
}


$ cat /etc/shinken/modules/webui2.cfg
[...]
modules     ui-graphite2
[...]
```

Shinken will automatically replace YOURSERVERNAME with the broker hostname at runtime to try and make it work for you, but you MUST change it to the appropriate value.

**Note**: the source code of this `ui-graphite2` module is located in the `refactored` branch of the `ui-graphite` repository.