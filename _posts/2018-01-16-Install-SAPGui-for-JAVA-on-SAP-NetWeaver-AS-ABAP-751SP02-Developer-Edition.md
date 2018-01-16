---
layout: post
title: Install SAP GUI for JAVA 
description: Install SAP GUI for JAVA on SAP NW751 SP02 Developer Edition.
date: 2018-01-16 19:44:00+0100
date_modified: 2018-01-16 19:44:00+0100
categories: [sap, sapgui, java]
tags:
  - sap
  - sap netweaver
  - java
  - sapgui
author: wechris
image:
  path: /img/2018-01-16-Install-SAPGui-for-JAVA-on-SAP-NetWeaver-AS-ABAP-751SP02-Developer-Edition/sapgui4java.jpg
  width: 629
  height: 600
---
##### Install SAP GUI for JAVA on Vagrant Box SAPNW751SPS02

Go to client folder > JavaGUI (LINUX, MacOSX, Windows) and execute the jar file of your OS to install the client.

{% include lightbox.html src="/img/2018-01-16-Install-SAPGui-for-JAVA-on-SAP-NetWeaver-AS-ABAP-751SP02-Developer-Edition/files.jpg" title="SAP GUI installation" width="500" %}

Click on next and follow the installation wizard.

{% include lightbox.html src="/img/2018-01-16-Install-SAPGui-for-JAVA-on-SAP-NetWeaver-AS-ABAP-751SP02-Developer-Edition/intro.jpg" title="SAP GUI installation" width="500" %}

{% include lightbox.html src="/img/2018-01-16-Install-SAPGui-for-JAVA-on-SAP-NetWeaver-AS-ABAP-751SP02-Developer-Edition/readme.jpg" title="SAP GUI installation" width="500" %}

{% include lightbox.html src="/img/2018-01-16-Install-SAPGui-for-JAVA-on-SAP-NetWeaver-AS-ABAP-751SP02-Developer-Edition/options.jpg" title="SAP GUI installation" width="500" %}

{% include lightbox.html src="/img/2018-01-16-Install-SAPGui-for-JAVA-on-SAP-NetWeaver-AS-ABAP-751SP02-Developer-Edition/summary.jpg" title="SAP GUI installation" width="500" %}

##### SAP GUI for JAVA logon

When connecting with **127.0.0.1:3200** on the Host machine, there is a port forwarding in VirtualBox to **10.0.2.15:3200** in Guest machine (SAP NetWeaver server). Therefore, open SAP GUI for JAVA, and add the following connection in __expert mode__:
- **conn=/H/127.0.0.1/S/3200**

{% include lightbox.html src="/img/2018-01-16-Install-SAPGui-for-JAVA-on-SAP-NetWeaver-AS-ABAP-751SP02-Developer-Edition/sapguiconfig.jpg" title="SAP GUI Java connection" width="500" %}

##### Success!

{% include lightbox.html src="/img/2018-01-16-Install-SAPGui-for-JAVA-on-SAP-NetWeaver-AS-ABAP-751SP02-Developer-Edition/sapgui4java.jpg" title="SAP GUI Java connection" width="500" %}