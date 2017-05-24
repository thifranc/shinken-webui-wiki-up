# Shinken Web User Interface

## Important information

As you may have noticed, the activity on the Shinken framework is very low since several months :/ Because of these, I do not want to invest more effort for releasing new Web UI versions. I had to take this decision because, for my own needs, I decided to migrate to another monitoring framework.
 
 As of today, I realize that I do not have enough time to maintain the Shinken WebUI with an aceceptable quality. Hence, I prefer to abandon the maintenance of this project. I am sincerely sorry for the inconvenience this may cause to you.



*Please note that the screen captures used to illustrate some pages are not up to date with the most recent version!*

Shinken provides a self sufficient Web User Interface, which includes its own web server (No need to setup any other Web server). The WebUI is built with the most recent Javascript Bootstrap 3 library as to deliver a user interface compatible with a wide range of Web browsers.

It has been tested successfully with the last Shinken framework versions. As of today, it has been tested with Shinken 2.4.0 and 2.4.1 versions.

![Screenshots](https://github.com/shinken-monitoring/mod-webui/blob/master/doc/animation.gif)

The Shinken WebUI is started at the same time as the Shinken broker. It is configured by setting a few basic parameters in several modules.

WebUI is built upon a main application and it uses several modules to delegate some actions:
- authentication modules, used to authenticate users that log in
- storing modules, used to make parameters persistent
- graphing modules, used to display graphs built from metrics
- logs module, used to make available hosts/services activity
- availability module, used to compute hosts availability

**The new WebUI embeds its own modules for authentication, storage, logs and availability, whereas simplifying installation and configuration.**

