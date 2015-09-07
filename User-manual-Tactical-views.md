## Minemap
The minemap page is a view of the monitored hosts and services in a matrix (you know the famous minemap game ...).


 <img src="https://raw.githubusercontent.com/wiki/shinken-monitoring/mod-webui/09.jpg" width="768">


## Worldmap
The worldmap page is a view of the monitored hosts on a Google map.

Each host is located on the world map with a marker colored depending upon host overall state. Depending upon the zoom level, hosts are grouped in a cluster which color depends upon all included hosts overall state.

Host overall state is computed as is:

- if host state is not Up, orange/red color for unreachable/down

- if host state is Up, 

   - if at least one service is Critical, red color
   - if at least one service is Warning, orange color
   - if all services are Ok, green color


 <img src="https://raw.githubusercontent.com/wiki/shinken-monitoring/mod-webui/10.jpg" width="768">



