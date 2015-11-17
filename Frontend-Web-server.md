#  Standalone Web UI

The Web UI embeds its own Web server [Python Bottle](http://bottlepy.org/docs/dev/index.html). This built-in non-threading HTTP server is based on wsgiref WSGIServer may become a performance bottleneck when server load increases. It is recommended, at least, to install an alternative multi-threaded server like [CherryPy](http://www.cherrypy.org/) which is also recommended as a dependency for the Shinken framework.

A simple installation of the Python package is enough and no extra-configuration is necessary fot CherryPy to be used as a backend server for the Web UI.
```
pip install CherryPy
```

# Using nginx as a frontend server
Using a reverse proxy server as a frontend for the Web UI allows to improve performances especially for static files (javascript, css, ...). The configuration is very easy with nginx.
```
   # Install nginx
   apt-get install nginx

   # Delete default virtual host
   rm /etc/nginx/sites-available/default

   # Create and activate a new virtual host
   vi /etc/nginx/sites-available/shinken
   => See below for file content ... choose HTTP or HTTPS configuration file.

   # Activate the new virtual host
   ln -s /etc/nginx/sites-available/shinken /etc/nginx/site-enabled/shinken

   # Restart nginx
   /etc/init.d/nginx restart
```

## nginx HTTP configuration
This configuration file may be used if you need to change the Web UI port (http://shinkenmain).

```
# /etc/nginx/sites-available/shinken
# Shinken WebUI
#
server {
    # IPv4 support
    listen 80;
    # IPv6 support
    #listen [::]:80;

    server_name shinken;

    access_log          /var/log/nginx/shinken_access.log;
    error_log           /var/log/nginx/shinken_error.log;

    # Avoid robots
    location /robots.txt {
        return 200 "User-agent: *\nDisallow: /";
    }

    # Serve static content directly
    location /static/(.*\/)? {
        try_files htdocs/$uri plugins/$1/htdocs/$uri @webui;
    }
    location @webui {
        root /var/lib/shinken/modules/webui/;
    }

    # Redirection
    location / {
        # Set the adequate variables so that the WebUI will
        # know what hostname it has, this is useful for redirects
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   Host      $http_host;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;

        # Replace 7767 (default) by the port your shinken WebUI is listening on.
        proxy_pass http://localhost:7767;
        proxy_read_timeout  60;

        proxy_redirect http://localhost:7767 http://shinkenmain:80;
    }
}
```

## nginx HTTPS configuration
This configuration file may be used if you need to use the Web UI with a secured SSL connection (https://shinkenmain).

```
# /etc/nginx/sites-available/shinken_ssl
# Shinken WebUI
#
server {
    # IPv4 support
    listen 443;
    # IPv6 support
    #listen [::]:443;

    server_name shinken;

    access_log           /var/log/nginx/shinken_access.log;
    error_log            /var/log/nginx/shinken_error.log;

    ssl on;
    ssl_certificate      /etc/nginx/ssl/shinken.crt;
    ssl_certificate_key  /etc/nginx/ssl/shinken.key;

    # Avoid robots
    location /robots.txt {
        return 200 "User-agent: *\nDisallow: /";
    }

    # Serve static content directly
    location /static/(.*\/)? {
        try_files htdocs/$uri plugins/$1/htdocs/$uri @webui;
    }
    location @webui {
        root /var/lib/shinken/modules/webui/;
    }

    # Redirection
    location / {
        # Set the adequate variables so that the WebUI will
        # know what hostname it has, this is useful for redirects
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   Host      $http_host;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;

        # Replace 7767 (default) by the port your shinken WebUI is listening on.
        proxy_pass http://localhost:7767;
        proxy_read_timeout  60;

        proxy_redirect http://localhost:7767 https://shinkenmain:443;
    }
}
```


## nginx HTTP configuration, relocate in sub folder
This configuration file may be used if you need to change the Web UI port and to relocate the aplication to a sub folder (http://shinkenmain/shinken).

**Note:** This configuration requires a recent version of nginx (min. 1.9.4). You will probably need to install the nginx-full package from the jessie-backport repositories, or follow [this tutorial on nginx site](https://www.nginx.com/resources/wiki/start/topics/tutorials/install/).

```
# /etc/nginx/sites-available/shinken
# Shinken WebUI
#
server {
    # IPv4 support
    listen 80;
    # IPv6 support
    #listen [::]:80;
    server_name shinkenmain;

    access_log  /var/log/nginx/shinken_sub_access.log;
    error_log  /var/log/nginx/shinken_sub_error.log;

    # Serve static content directly
    location /static/(.*\/)? {
        try_files htdocs/$uri plugins/$1/htdocs/$uri @webui;
    }
    location @webui {
        root /var/lib/shinken/modules/webui/;
    }

    location ~* ^/(all|forms|inner|static|dashboard|availability|logs|widget|cv|user|modal|gotfirstdata|host/cv) {
        return 301 /shinken$request_uri;
    }

    location /shinken/ {
        # Set the variables so that the WebUI will
        # know what hostname it has, this is useful for redirects
        proxy_set_header   X-NginX-Proxy true;
        proxy_set_header   X-Real-IP $remote_addr;
        proxy_set_header   Host      $http_host;
        #proxy_set_header   Host      $host;
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header   X-Forwarded-Proto $scheme;


        # Replace 7767 (default) by the port your shinken webui is listening on
        proxy_pass http://localhost:7767/;
        proxy_redirect default;

        # Sub_filter all the occurrences of the page
        sub_filter_once off;

        # All patterns that should be rewritten
        sub_filter "href=\"/" "href=\"/shinken/";
        sub_filter "=\"/static" "=\"/shinken/static";
        sub_filter "=\"/" "=\"/shinken/";
    }
}
```