# User's guide

## Forewords

Shinken Web User Interface is built with a modern CSS and Javascript library (Bootstrap 3) to allow access with most Web browser even on small devices or tablets.

## Pre-requisites

It is important to be aware of the monitoring strategies that are used to monitor the hosts/services. Especially if the hosts/services are monitored using passive mode. 

This document assumes that the reader is aware of the monitoring behavior and is familiar with the Nagios/Shinken terminology about monitoring objects: hosts, services, timeperiods, ...

## Logging in

On the application login page, log in with the provided username and password. If the credentials are accepted and access is granted, the user is relocated on the Web UI main page. 

<img src="https://raw.githubusercontent.com/wiki/shinken-monitoring/mod-webui/01.jpg" width="768">

## Application main page

### Application layout

The application main page is built upon a layout made of:
- an header area, on top of page, containing a navigation bar, a filter box, ...
- a left side area, containig a user menu to navigate among the application pages
- a footer area, including the version and legals

<img src="https://raw.githubusercontent.com/wiki/shinken-monitoring/mod-webui/02.jpg" width="768">

The footer is solely intended for information about the current application version and copyright, and do not have any other function.

The sidebar menu allows to choose among the application pages. Some menu items are multi levels and have to be clicked to access to more precise choice.

The header bar contains information about the current application context and status.

### Header bar

The header area on top of page contains:

- the application logo, which displays a modal box with information about the application
- a filters button, to access some predefined filters usable on some application pages as problems, minemap, ...
- a filter edit box that allows to edit a filter for displayed objects
- an overall hosts status icon
- an overall services status icon
- an icon to access the "one eye dashboard"
- an indicator icon for the page refresh status
- the connected user information / settings

> The filtering feature of the header bar is developed in [the Problems chapter](https://github.com/shinken-monitoring/mod-webui/wiki/User-manual-Problems).

Clicking on the application logo pops up a modal box that contains informations about the current application version and Shinken version.

### Sidebar menu

The sidebar menu is organised as is:
- Dashboard, main application page, [described here](https://github.com/shinken-monitoring/mod-webui/wiki/User-manual-Dashboard)
- Problems, the current problems view, [described here](https://github.com/shinken-monitoring/mod-webui/wiki/User-manual-Problems)
- Groups and tags, [described here](https://github.com/shinken-monitoring/mod-webui/wiki/User-manual-Groups):
  - Hosts groups
  - Services groups
  - Hosts tags
  - Services tags
- Tactical views, [described here](https://github.com/shinken-monitoring/mod-webui/wiki/User-manual-Tactical-views):
  - Impacts
  - Minemap
  - World map
  - Wall
- System, [described here](https://github.com/shinken-monitoring/mod-webui/wiki/User-manual-System):
  - Configuration
  - Status
  - Logs
  - Availability
- Configuration, [described here](https://github.com/shinken-monitoring/mod-webui/wiki/User-manual-Configuration):
  - Parameters
  - Contacts
  - Contacts groups
  - Commands
  - Timeperiods

