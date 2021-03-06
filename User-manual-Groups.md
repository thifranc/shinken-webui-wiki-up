## Contacts groups
The contacts groups page is a view of the defined contacts in the monitoring system.

Each contacts group contains the name (and a link to ...) of the contacts declared as member for the group.

Selecting the contacts group name links to the *All resources* page, with a filter on the contacts group.

Selecting the name of a contact links to the contact information page.


## Hosts groups
The hosts groups page is a view of the monitored hosts grouped by hosts groups. This page displays the groups of the current level, starting from Level 0 groups.

The Web UI builds an hosts group hierarchy based on the membership of each group. As of it, each group gets a level depending upon its position in this hierarchy. Groups that are not member of any other group are Level 0 groups. 

Each hosts group contains information : 
 - on the left side : Up, Unreachable, Down and Unknown hosts status.
 - on the right side : number of hosts and groups included

Selecting the group name links to the *All resources* page, with a filter on the selected hosts group, displaying all the hosts that are members of the group.

Selecting the hosts state icon links to the *All resources* page, with a filter on the selected hosts group and state, displaying all the hosts that are members of the group and in the selected state.

 <img src="https://raw.githubusercontent.com/wiki/shinken-monitoring/mod-webui/06.jpg" width="768">

**The same logic is applicable for services groups!**

## Hosts tags
The hosts tags page is a view of the monitored hosts grouped by hosts tags. This view is switchable from box to list thanks to the upper right icons.

Each hosts group contains information about the Up, Unreachable, Down and Unknown hosts status.

Selecting the tags group name links to the *All resources* page, with a filter on the selected tag, displaying all the hosts that are tagged with the selected tag.

 <img src="https://raw.githubusercontent.com/wiki/shinken-monitoring/mod-webui/07.jpg" width="768">

**The same logic is applicable for services tags!**