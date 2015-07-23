## Minemap
The minemap page is a view of the monitored hosts and services in a matrix (you know the famous minemap game ...).


![Minemap page](./Capture05.JPG "Minemap")


## Worldmap
The worldmap page is a view of the monitored hosts on a Google map.

Each host is located on the world map with a marker colored depending upon host overall state. Depending upon the zoom level, hosts are grouped in a cluster which colors depend upon all included hosts overall state.

Host overall state is computed as is:

- if host state is not Up, orange/red color for unreachable/down

- if host state is Up, 

   - if at least one service is Critical, red color
   - if at least one service is Warning, orange color
   -  if all services are Ok, green color


![Worldmap page](./Capture06.JPG "Worldmap")

Note that it is possible to view an OSM map layout instead of Google's one ...


