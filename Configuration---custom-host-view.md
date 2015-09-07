## Custom host view configuration

The Web UI host view allows to display custom views for an host.

Custom views are intended to be delivered by installed packs (linux-ssh, ...) but very few packs provide such custom views. 

To compensate for the few available custom views, the Web UI provides its own generic and configurable view. This embedded custom view uses a set of customizable parameters.

to compensate for missing custom views in some packs, the Web UI allows to replace a missing custom view. this replacement features uses another set of parameters.


### Web UI custom view configuration

How-to:
- add the *custom_views* property to an host to activate the host custom view
- the required views are a comma separated list
- each element in the list may contain a */config* part to specify the configuration file to use

In this example:
```
   custom_views                 host,host/default2
```
the Web UI will create two views:
- the first one uses the *default.cfg* configuration file 
- the second one uses the *default2.cfg* configuration file 

The custom views configuration files are located in the `/var/lib/shinken/modules/webui2/plugins/cv_host` directory. As an example, the `default.cfg` file is built as is:
```
[view]
# Load service
# - match if Shinken service description contains 'svc_load_name' (Python regexp)
svc_load_name=load|Load
svc_load_used=^load1$|^load5$|^load15$
svc_load_uom=

# CPU service
# - match if Shinken service description contains 'svc_cpu_name' (Python regexp)
svc_cpu_name=cpu|CPU|Linux procstat
svc_cpu_used=^cpu_all_idle|cpu_all_iowait|cpu_all_usr|cpu_all_nice|cpu_idle|cpu_iowait|cpu_usr|cpu_nice|total 30s|total 1m|total 5m
svc_cpu_uom=%|c

# Disks service
# - match if Shinken service description contains 'svc_dsk_name' (Python regexp)
svc_dsk_name=^disk|disks|Disks|Partitions$
svc_dsk_used=used_pct|^/(?!dev|sys|proc|run?)(.*)$
svc_dsk_uom=^%|(.?)B$

# Memory service
# - match if Shinken service description contains 'svc_mem_name' (case insensitive)
svc_mem_name=memory|Memory
svc_mem_used=^(.*)$
svc_mem_uom=^%|(.?)B$

# Network service
# - match if Shinken service description contains 'svc_net_name' (case insensitive)
svc_net_name=network|NET Stat|Network
svc_net_used=eth0_rx_by_sec|eth0_tx_by_sec|eth0_rxErrs_by_sec|eth0_txErrs_by_sec|eth0_in_Bps|eth0_out_Bps|BytesReceived|BytesSent
svc_net_uom=p/s|(.*)
```


### hosts related properties

To specify custom views to be used for an host, the WebUI uses the `custom_views` property defined in the host definition.

Host definition:
```
define host{

   ... 
   # Specify a custom view to be used
   custom_views         host
   ...
   # Specify several custom views to be used
   custom_views         linux_cpu,linux_memory
   ...
   # Add several custom views to an host
   custom_views         +linux_ssh,linux_ssh_memory,linux_ssh_processes
}
```

> The `custom_views` property definitions are often used in an host template instead of the host definition ... this is why you can *add* custom views to an host.
