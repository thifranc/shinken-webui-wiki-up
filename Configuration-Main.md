## Configuring the main application

The WebUI configuration file (*webui2.cfg*) is located in the */etc/shinken/modules* directory. This file is largely commented and is quite self explanatory.

```
## Module:      WebUI
## Loaded by:   Broker
# The Shinken web interface and integrated web server.
define module {
   module_name         webui
   module_type         webui


   ## Modules for WebUI
   ## User authentication:
   # - auth-cfg-password (internal) : Use the password set in Shinken contact for auth.
   # - auth-htpasswd (internal)     : Use an htpasswd file for auth backend.
   # You may remove the modules 'auth-cfg-password' and 'auth-htpasswd' from your
   # configuration because the WebUI embeds those authentication methods.
   # - auth-ws-glpi                 : Use the Glpi Web Services for user authentication
   # - auth-active-directory        : Use AD for auth backend (and retrieve photos).
   
   # htpasswd (apache like) file containing username/passwords
   # Use an Apache htpasswd file or build your own (http://www.htaccesstools.com/htpasswd-generator/)
   htpasswd_file              /etc/shinken/htpasswd.users

   ## Graphs:
   # - ui-pnp                : Use PNP graphs in the UI.
   # - ui-graphite           : Use graphs from Graphite time series database.
   
   ## Storage:
   # - mongodb               : Save user preferences to a Mongodb database
   #                         : Get hosts/services availability from a Mongodb database
   #                         : Get Shinken logs and hosts history from a Mongodb database
   # - SQLitedb              : Save user preferences to a SQLite database
   
   ## Hosts/services availability:
   # - mongo-logs            : Store hosts/services availability in a Mongodb database
   #                         : Display hosts/services availability
   #                         : Display Shinken logs
   
   ## Helpdesk:
   # - glpi-helpdesk         : Get hosts information from an helpdesk application
   #                         : Notify helpdesk for hosts problems
   modules                    


   # Web server configuration
   host                       0.0.0.0      ; All interfaces = 0.0.0.0
   port                       7767


   # Authentication secret for session cookie
   auth_secret                CHANGEME
                              ; CHANGE THIS or someone could forge cookies


   # WebUI information
   # Overload default information included in the WebUI
   #about_version              2.0 alpha
   #about_copyright            (c) 2013-2015 - License GNU AGPL as published by the FSF, minimum version 3 of the License.
   #about_release              Bootstrap 3 User Interface


   # Configuration directory
   config_dir                 /var/lib/shinken/config/

   # Share directory
   share_dir                  /var/lib/shinken/share/

   # Photos directory
   photos_dir                 /var/lib/shinken/share/photos/

   # For external plugins to load on webui
   #additional_plugins_dir



   # Login form
   # Welcome text in the login form.
   login_text                 Login to the Shinken WebUI - Live System

   # Company logo in the login form and header bar
   # company_logo property is suffixed with .png and searched in photos_dir
   # Default logo is used if company_logo is not found in photos_dir
   # Default logo is always used if company_logo property is empty
   # Default logo is default_company.png (Shinken logo) in webui/htdocs/images
   company_logo               my_company


   allow_html_output          1
                              ; Allow or not HTML chars in plugins output.
                              ; WARNING: Allowing can be a security issue.

   tag_as_image               0
                              ; Use image if available for elements' tags
                              ; Monitoring packs may include an image for the host/service tag
                              ; WebUI also has some tags as images

   play_sound                 0
                              ; Play sound on new non-acknowledged problems.

   # Gravatar image for logged in users
   gravatar                   0
                              ; If gravatar=0, image used is username.png in webui/htdocs/images/logo
                              ; If not found, default is default_user.png in webui/htdocs/images/logo

   # Refresh period
   refresh_period             60
                              ; Number of seconds between each page refresh
                              ; 0 to disable refresh

   # Visual alerting thresholds
   # Used in the dashboard view to select background color for percentages
   hosts_states_warning       95
   hosts_states_critical      90
   services_states_warning    95
   services_states_critical   90

   # WebUI timezone (default is Europe/Paris)
   #timezone                  Europe/Paris



   # Manage contacts ACL
   # 0 allows actions for all contacts
   # 1 allows actions only for contacts whose property 'is_admin' equals to 1
   # Default is 1
   manage_acl                 1

   # Allow anonymous access for some pages
   # 0 always disallow
   # 1 allows anonymous access if an anonymous
   # contact is declared in the Shinken configuration
   # Default is 0
   allow_anonymous            1



   ## Advanced Options for Bottle Web Server
   # Best choice is auto, whereas Bottle chooses the best server it finds amongst:
   # - [WaitressServer, PasteServer, TwistedServer, CherryPyServer, WSGIRefServer]
   # Install CherryPy for a multi-threaded server ...
   # ------------
   # Handle with very much care!
   #http_backend              auto
                              ; Choice is: auto, wsgiref or cherrypy if available

   # Specific options store in the serverOptions when invoking Bottle run method ...
   # ------------
   # Handle with very much care!
   #bindAddress               auto
                              ; bindAddress for backend server
   #umask                     auto
                              ; umask for backend server

   #remote_user_enable        1
                              ; If WebUI is behind a web server which
                              ; has already authentified user, enable.

   #remote_user_enable        2
                              ; Look for remote user in the WSGI environment
                              ; instead of the HTTP header. This allows
                              ; for fastcgi (flup) and scgi (flupscgi)
                              ; integration, eg. with the apache modules.

   #remote_user_variable      X_Remote_User
                              ; Set to the HTTP header containing
                              ; the authentificated user s name, which
                              ; must be a Shinken contact.
}
```

## Web server configuration
 *host* and *port* parameters allow a simple configuration for the address and port on which the WebUI will be available.

 The user authentication is based on a cookie which is protected thanks to the *auth_secret* parameter. You are strongly invited to change this value right after installing the application!

 The following parameters are not intended to be changed except for advanced users with specific Web server configuration: 
- http_backend              auto
- bindAddress               auto
- umask                     auto
- remote_user_enable        1

## Modules 
 *modules* parameter allows to declare the available modules to the application. For more information, see [configuring the application.](https://github.com/shinken-monitoring/mod-webui/wiki/Configuration)


## Directories

 The configuration directory (*config_dir*) is used by the application to store and reload the user's preferences if there is not any other module available. The preferences are stored as Json objects in text files in the *config_dir* directory.

> Take care that this directory must be allowed full access to the Shinken user.

 The share directory (*share_dir*) is used by the Shinken packs to expose data to the Web UI. Currently, the packs are copying images for the hosts/services tags. For a tag **pack**, the Web UI searches in *share_dir/images/sets/pack/tag.png* to find an image to use for the tag.

 The photos directory (*photos_dir*) is used by the application to retrieve the company logo used on the login page / header bar and the logged in users photos displayed in the user profile.

 The company logo is searched in *photos_dir* as a file named *default_company.png*. If not found, the application uses its own logo: 

 <img src="https://raw.githubusercontent.com/wiki/shinken-monitoring/mod-webui/logo_very_small.png" width="128">

   
 When loading its plugins, the application scans the additional plugins directory (*additional_plugins_dir*) to load extra plugins. The scanning for this directory occurs before the modules loading and the Web UI plugins loading. As of it, the extra plugins will have the lead on the Web server routes ...

> Currently, this feature is not used ... as we may know :-)

## Application time zone

 The application is handling a time zone for some features (acknowledgements, downtimes scheduling, ...). You are invited to declare your current time zone in the *timezone* parameter. Default value is *Europe/Paris*.

## Application information

The application about box is displayed when the user clicks on the company logo in the header bar. The information contained in this box are included in the application but it is possible to overload these information with configuration parameters.

 The configuration parameters *about_version*, *about_copyright* and *about_release* overload their respective values declared in the application.

 > Unless you really need to display your own information box it is not recommended to use these parameters.


## User management

 The *manage_acl* property allows to inhibit the user rights management. It is not recommended to change this value because all users enabled to log in will be allowed to execute all commands on all the monitored resources.

The *allow_anonymous* property allows to grant access to anonymous users on some application URI. Currently, the only 'opened' URI is the "one eye" dashboard view.

This feature implies that:
- the *allow_anonymous* property is set to 1
- an anonymous contact is declared in the Shinken configuration as below

```
# This is an anonymous contact ... used to allow access to some pages without any login !
define contact{
   use                  generic-contact
   contact_name         anonymous
   alias                Anonymous
   email                nobody@localhost
   is_admin             1
   can_submit_commands  0
}
```

## Visual alerting thresholds

 The *hosts_states_warning*, *hosts_states_critical*, *services_states_warning* and *services_states_critical* parameters are used in dashboard (or similar) to color a background or test depending upon a percentage of bad status.


## Application parameters

 The *allow_html_output* and *max_output_length* are used for displaying the checks output content. They define if HTML is allowed and what is the maximum size that will be displayed.

 The *tag_as_image* parameter determines if an image may be used if available for hosts and services tags.

 If the *gravatar* parameter is set to 1, the application tries to find a gravatar image for the logged in user.

 The *refresh_period* is the number of seconds between each page refresh.
