The WebUI can use external modules to check user/password before allowing access to the interface. The WebUI also embeds two authentication modules (contact password and htpasswd file) to avoid installing some extra modules.

The user authentication is made by the WebUI login process in the following order: 
- first, tries to authenticate the user with the extra modules defined in its configuration
- then, tries the *Contact file* embedded authentication
- last, tries the *htpasswd file* embedded authentication

> You can use as many authentication modules as you want. For instance, you can combine the default config file authentication with an active directory.

## Default authentication: Shinken contact - (was auth-cfg-password)

This module is the most simple one and the weakest one also because it uses a password stored in the contacts files. The user is authenticated if the password is identical to the one stored in the file.

```
define contact{
    use             generic-contact
    contact_name    admin
    alias           Administrator
    email           admin@localhost
    password        admin
    is_admin        1
    expert	    1
}
```

**You don't need to install and configure this module as it is now embedded in the webui and used by default.** If you don't want to use it, just don't put passwords in you contact definitions.

## Default authentication: Apache htpasswd - (was auth-htpasswd)

This module uses an Apache passwd file (htpasswd) as authentification backend. All it needs is the full path of the file.

Check that the owner of the file is the defined Shinken user and that this file is readable. 

> If you don't have such a file you can generate one with the “htpasswd” command (in Debian's “apache2-utils” package), or from websites like htaccessTools (http://www.htaccesstools.com/htpasswd-generator/). 

**You don't need to install and configure this module as it is now embedded in the webui and used by default.** If you don't want to use it, just leave the htpasswd file name empty in the WebUI configuration file.

How to configure:
```
$ cat /etc/shinken/modules/webui.cfg
[...]

   
   # htpasswd (apache like) file containing username/passwords
   # Use an Apache htpasswd file or build your own (http://www.htaccesstools.com/htpasswd-generator/)
   htpasswd_file              /etc/shinken/htpasswd.users

```

## Active Directory / OpenLDAP - (auth-active-directory)

This module allows to lookup passwords into both Active Directory or OpenLDAP entries.

How to install:
```
$ shinken install auth-active-directory
```

How to configure:
```
$ cat /etc/shinken/modules/auth_active_directory.cfg
[...]

## Module:      auth-active-directory
## Loaded by:   WebUI
## Usage:       Uncomment and set your value in ldap_uri
# Check authentification for WebUI using an Active Directory server.
define module {
    module_name     auth-active-directory
    module_type     ad_webui
    #ldap_uri        ldaps://myserver
    username        user
    password        password
    basedn          DC=google,DC=com

    # For mode you can switch between ad (active dir)
    # and openldap
    mode            ad
}
```

Change “adserver” by your own dc server, and set the “user/password” to an account with read access on the basedn for searching the user entries.

Change “mode” from “ad” to “openldap” to make the module ready to authenticate against an OpenLDAP directory service.


## Glpi user - (auth-ws-glpi)

This module allows to authenticate a Web UI user against a Glpi (http://www.glpi-project.org/) database.

Default configuration needs to be tuned up to your Glpi configuration. 

At first, you need to activate and configure the GLPI WebServices to allow connection from your Shinken server.
Then you need to set the WS URI (uri) parameter in the configuration file.

Each time a user will try to log in in the Web UI, the module will try to connect with username and password. If connection is successful the user will be logged in the Web UI.

> Please note that a Shinken contact must exist with the same name as the username ! This contact may be a locally flat file contact or a contact fetched from Glpi thanks to import-glpi module.
      

How to install:
```
$ shinken install auth-ws-glpi
```

How to configure:
```
$ cat /etc/shinken/modules/auth-ws-glpi.cfg
[...]

## Module:      auth-cfg-password
## Loaded by:   WebUI
# Check authentification using login/password in Glpi database
define module {
    module_name     auth-ws-glpi
    module_type     auth-ws-glpi

    # Glpi Web service URI
    # Assuming Glpi is located on the same server in the glpi directory
    uri             http://glpi085/glpi/plugins/webservices/xmlrpc.php
}

$ cat /etc/shinken/modules/webui.cfg
[...]
modules     auth-ws-glpi
[...]
```

## Others

There may be other authentication modules. Just search for "auth" in https://github.com/shinken-monitoring/ repositories or on https://shinken.io/.