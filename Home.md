# Shinken Web User Interface

*Pleaase note that the screen captures used to illustrate some pages are not up to date with the most recent version!*

Shinken can provide a self sufficient Web User Interface, which includes its own web server (No need to setup any other Web server)

![Screenshots](https://github.com/shinken-monitoring/mod-webui/blob/bs3/doc/images/animation.gif)

The Shinken WebUI is started at the same time as the Shinken broker. It is configured by setting a few basic parameters in several modules.

WebUI is built upon a main application and it uses several modules to delegate some actions:
- authentication modules, used to authenticate users that log in
- storing modules, used to make parameters persistent
- graphing modules, used to display graphs built from metrics
- logs module, used to make available hosts/services activity
- availability module, used to compute hosts availability

The most recent WebUI embeds its own modules for authentication, storage, logs and availability, whereas simplifying installation and configuration.
