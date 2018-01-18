---
layout: post
title: Create Data for Flight Data Model 
description: How to generate standard entries in the database tables that belong to the flight data model.
date: 2018-01-18 20:58:59+0100
date_modified: 2018-01-18 20:58:59+0100
categories: [sap,table,sflight]
tags:
  - sap
  - table
  - sflight
author: wechris
image:
  path: /img/2018-01-18-Create-Data-for-Flight-Data-Model/report.jpg
  width: 629
  height: 600
---
### Flight Model

The flight model is the base of all ABAP examples and in the most SAP courses and tutorials. All the tables mentioned in the course matirials exists in your system, so you can reproduce the examples directly in the system.

The flight model is based on data model BC_TRAVEL. :airplane:

The most important tables of the flight model are: 

* T000: Client table
* SCURX: Currencies (key: currency key)
* SBUSPART: Business partner (key: client, partner number)
* STRAVELAG: Travel agencies (key: client, travel agency number)
* SCUSTOM: Customers (key: client, customer number)
* SCARR: Carriers (key: client, carrier ID)
* SCOUNTER: Sales counters (key: client, carrier ID, sales counter number)
* SPFLI: Flight schedule (key: client, carrier ID, connection number)
* SFLIGHT: Flights (key: client, carrier ID, connection number, date of flight)
* SBOOK: Flight bookings (key: client, carrier ID, connection number, date of flight, booking number, customer number)

Run the following report in <code>SE38</code> transaction:

<code>SAPBC_DATA_GENERATOR</code>

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/report.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/report_settings.jpg" title="SFLIGHT" width="500" %}

Run the report <code>F8</code>:

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/reportlog.jpg" title="SFLIGHT" width="500" %}

#### Result
350 entries in the table ```SFLIGHT``` is OK, but a little bit more makes more fun.

The large data records can only be created in the background.

### Backgroundjob to create Flight Model

Background job is a non-interactive process that runs behind the normal interactive operations. They run in parallel and do not disturb interactive (foreground jobs) processes and operations.

It is scheduled from <code>SM36</code>. You can analyze it from <code>SM37</code> by viewing its job log.

I used the Job Wizzard:

Run the following transaction <code>SM36</code>:

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard01.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard02.jpg" title="SFLIGHT" width="500" %}

I used the Maximum Data Record

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard03.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard04.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard05.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard06.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard07.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard08.jpg" title="SFLIGHT" width="500" %}

Check the background job:

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/jobwizard09.jpg" title="SFLIGHT" width="500" %}


#### Check the result of table SFLIGHT

Run the following report in <code>SE38</code> transaction:

<code>RSTABLESIZE</code>

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/chktable00.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/chktable01.jpg" title="SFLIGHT" width="500" %}

{% include lightbox.html src="/img/2018-01-18-Create-Data-for-Flight-Data-Model/chktable02.jpg" title="SFLIGHT" width="500" %}

__1.321__ entries in the table ```SFLIGHT``` perfect :smiley:.