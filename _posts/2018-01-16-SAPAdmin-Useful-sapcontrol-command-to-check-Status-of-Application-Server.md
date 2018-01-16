---
layout: post
title: Useful sapcontrol commands 
description: Useful sapcontrol commands to check Status of Application server.
date: 2018-01-16 20:44:00+0100
date_modified: 2018-01-16 20:44:00+0100
categories: [sap, sapcontrol, sapadmin]
tags:
  - sap
  - sapcontrol
  - sapadmin
author: wechris
image:
  path: /img/2018-01-16-SAPAdmin-Useful-sapcontrol-command-to-check-Status-of-Application-Server/GetVersionInfo.jpg
  width: 629
  height: 600
---
##### Useful sapcontrol command to check Status of Application server

* sapcontrol -nr 00 -function GetVersionInfo

{% include lightbox.html src="/img/2018-01-16-SAPAdmin-Useful-sapcontrol-command-to-check-Status-of-Application-Server/GetVersionInfo.jpg" title="SAP GUI installation" width="500" %}

* sapcontrol -nr 00 -function GetProcessList

{% include lightbox.html src="/img/2018-01-16-SAPAdmin-Useful-sapcontrol-command-to-check-Status-of-Application-Server/GetProcessList.jpg" title="SAP GUI installation" width="500" %}

* sapcontrol -nr 00 -function GetSystemInstanceList

{% include lightbox.html src="/img/2018-01-16-SAPAdmin-Useful-sapcontrol-command-to-check-Status-of-Application-Server/GetSystemInstanceList.jpg" title="SAP GUI installation" width="500" %}

* sapcontrol -nr 00 -function ParameterValue < PARAMETER >
   * sapcontrol -nr 00 -function ParameterValue SAPLOCALHOST
   * sapcontrol -nr 00 -function ParameterValue PHYS_MEMSIZE
   * sapcontrol -nr 00 -function ParameterValue SAPPROFILE

{% include lightbox.html src="/img/2018-01-16-SAPAdmin-Useful-sapcontrol-command-to-check-Status-of-Application-Server/ParameterValue.jpg" title="SAP GUI installation" width="500" %}