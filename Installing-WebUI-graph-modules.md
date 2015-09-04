The WebUI can use external modules to display graphs corresponding to the services directly in the WebUI.

Currently there is two graphs systems supported : PNP4Nagios and Graphite.

> You can use as many graph modules as you want, but in most cases only one should be enough.

Note that the WebUI do not include graph's links directly, but will proxy the images. So you can configure PNP4Nagios and Graphite to listen only on localhost (or a limited set of IPs), and count on the WebUI to secure the access to these graphs (showing them only to authenticated and concerned users).

## PNP graphs

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

$ cat /etc/shinken/modules/webui.cfg
[...]
modules     ui-pnp
[...]
```

Shinken will automatically replace YOURSERVERNAME with the broker hostname at runtime to try and make it work for you, but you MUST change it to the appropriate value.


## Graphite graphs

You can ask for Graphite graphs with the `ui-graphite` module.


How to install:
```
$ shinken install ui-graphite
```

How to configure:
```
$ cat /etc/shinken/modules/ui-graphite.cfg
[...]

## Module:      ui-graphite
## Loaded by:   WebUI
# Use Graphite graphs in the WebUI, based on default or graphite URL API
# templates.
#
# IMPORTANT : Set the proper TIME_ZONE parameter in graphite : webapp/graphite/local_settings.py
# Set if to match the system setting.
# If not, 4h graphs will be broken.
define module {
    module_name     ui-graphite
    module_type     graphite-webui

    uri             http://YOURSERVERNAME

    templates_path  /etc/share/shinken/templates/graphite/

    # Graph configuration for dashboard widget
    dashboard_view_font   8
    dashboard_view_width  320
    dashboard_view_height 240

    # Graph configuration for element detail view
    detail_view_font      10
    detail_view_width     786
    detail_view_height    308

    # Optionally specify a source identifier for the metric data sent to
    # Graphite. This can help differentiate data from multiple sources for the
    # same hosts. HostA.GRAPHITE_DATA_SOURCE.service
    # You MUST set the same value in the graphite_perfdata and GRAPHITE_UI module
    # configuration.
    graphite_data_source    shinken
}

$ cat /etc/shinken/modules/webui.cfg
[...]
modules     ui-graphite
[...]
```

Shinken will automatically replace YOURSERVERNAME with the broker hostname at runtime to try and make it work for you, but you MUST change it to the appropriate value.

> Note that it exists a more recent version of this module that allows more subtle configuration and graph displays, such as grouping warning, critical, ... with the original metric ! Please feel free to request for this new version ... currently not yet published on Shinken.IO.