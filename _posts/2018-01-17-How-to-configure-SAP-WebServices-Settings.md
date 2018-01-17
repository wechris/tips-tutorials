---
layout: post
title: How to configure SAP WebServices Settings 
description: Technical Setup, Check and Reset of the SAP Web Service Runtime.
date: 2018-01-17 23:36:15+0100
date_modified: 2018-01-17 23:36:15+0100
categories: [sap,srt,webservice,icf,soap]
tags:
  - sap
  - srt
  - webservice
  - icf
  - soap
author: wechris
image:
  path: /img/2018-01-17-How-to-configure-SAP-WebServices-Settings/soamanager.jpg
  width: 629
  height: 600
---
### Configuring the Web Service Runtime

To enable the Web Services, you must have the Web service runtime configured.

#### Checking the Configuration

Using the report 'SRT_ADMIN_CHECK', you can check whether the Web service runtime is configured correctly.

{% include lightbox.html src="/img/2018-01-17-How-to-configure-SAP-WebServices-Settings/check.jpg" title="SAP GUI installation" width="500" %}

To get more details use the report 'SRT_ADMIN_ALL' with the option 'Check Technical Settings'

{% include lightbox.html src="/img/2018-01-17-How-to-configure-SAP-WebServices-Settings/report_check.jpg" title="SAP GUI installation" width="500" %}

{% include lightbox.html src="/img/2018-01-17-How-to-configure-SAP-WebServices-Settings/check_all.jpg" title="SAP GUI installation" width="500" %}

In each productive client and in client 000, execute the Run Technical Setup in transaction SRT_ADMIN_ALL. You can choose between the automatic and the manual setup.
Some parts of the technical setup have been automated. The report SRT_ADMIN_ALL provides this procedure when the option “Run Technical Setup” in block “Technical Configuration of Web Service Runtime” is selected and the report run is started (F8). For an easier problem solvement of inconsistencies, a reset  is also available.

{% include lightbox.html src="/img/2018-01-17-How-to-configure-SAP-WebServices-Settings/report_auto.jpg" title="SAP GUI installation" width="500" %}

The report generates a result page:

{% include lightbox.html src="/img/2018-01-17-How-to-configure-SAP-WebServices-Settings/admin_all.jpg" title="SAP GUI installation" width="500" %}

Now check the configuration again with the report 'SRT_ADMIN_ALL' with the option 'Check Technical Settings'

{% include lightbox.html src="/img/2018-01-17-How-to-configure-SAP-WebServices-Settings/check_all_after.jpg" title="SAP GUI installation" width="500" %}

or with the report 'SRT_ADMIN_CHECK'

{% include lightbox.html src="/img/2018-01-17-How-to-configure-SAP-WebServices-Settings/check_after.jpg" title="SAP GUI installation" width="500" %}

