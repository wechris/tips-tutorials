---
layout: post
title: Building a simple Gateway service 
description: Building a simple Gateway service.
date: 2017-11-19 14:44:00+0100
date_modified: 2017-11-19 14:44:00+0100
categories: [sap]
tags:
  - sap
  - sap netweaver gateway
  - gateway
  - gateway service
  - odata
author: wechris
image:
  path: /img/2017-11-19-building-your-first-simple-gateway-service/image19.png
  width: 629
  height: 600
---
1\. Purpose

SAP NetWeaver Gateway is an enabling component of SAP which allows you
to expose data from one or many systems to the outside world. The
underlying systems are usually SAP ones (SAP ECC, SAP CRM, SAP ERP and
so on. ) but it is important to note that you can use Gateway with
non-SAP systems too.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image1.png" title="VirtualBox" width="500" %}
 

Now you all have a basic understanding of what Gateway is, you're ready
to get on with the fun bit!

Pre-requisites for this exercise

-   SAP system (ERP / ECC / CRM)

-   SAP NetWeaver Gateway Release 2.0 Support Package 4 or above

    1.  Free Gateway [trial download
        here](http://scn.sap.com/community/developer-center/netweaver-gateway)

-   SAP AS ABAP 751 SP02 Developer Edition [download
    here](https://blogs.sap.com/2017/09/04/sap-as-abap-751-sp02-developer-edition-to-download/)

-   Some basic understanding of ABAP and databases

I use the SAP AS ABAP 751 SP02 Developer Edition with integrated Gateway. But I will explain as I go along how this would differ
if your Gateway instance is separate.

The data I will expose will be SFlight data, both the Flights (SFLIGHT)
and Carrier (SCARR) database tables. SFlight is a standard set of tables
and data which comes free with SAP systems. To find out more about
SFlight please visit this post.

2\. Create a new project using the Service Builder

Login to ERP and navigate to transaction SEGW

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image2.png" title="VirtualBox" width="500" %}

 

Click on "Create Project"

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image3.png" title="VirtualBox" width="500" %}

 

Chose a project name, give your project a description and select
Standard generation strategy.

In my example I will be creating the service as a local object.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image4.png" title="VirtualBox" width="500" %}

 

2.1 Create an entity "Carrier" from table SCARR

Right click on Data Model, and select Import \> DDIC Structure. 

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image5.png" title="VirtualBox" width="500" %}

Enter the name of the ABAP Dictionary Structure you want to use, and hit
enter. In our example we are using SCARR. 

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image6.jpeg" title="VirtualBox" width="500" %}   

 

Enter an Object Name, this will be the name of your entity. Select any
fields to not include (ignore) and ensure the correct key is selected.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image7.jpeg" title="VirtualBox" width="500" %}   

 

You can now change the names of any of the properties.

2.2 Create an entity set "Carriers"

Double click on "Entity Sets" on the left hand side, and chose "Append
Row". Enter an entity set name -- usually a pluralisation of the Entity
name, which in our example is Carriers.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image8.jpeg" title="VirtualBox" width="500" %}   

 

Chose the appropriate entity on which to base this entity set

2.3 Generate the Service

In order to use the service with the changes you have made you will
first need to "generate" it. This will create the underlying ABAP
classes for you, containing all the information you've specified thus
far in the service builder. Anytime you make a change in the service
builder you will need to generate again, to ensure the ABAP classes are
up-to date.

Select the project name that you wish to generate, and click the
generate wheel.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image9.png" title="VirtualBox" width="500" %}

 

Enter names which you want the underlying classes to have.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image10.png" title="VirtualBox" width="500" %}

 

It is best practice for the model provider class name to end in \_MPC
and the data provider class to end in \_DPC. The extension classes of
these should be named the same as their corresponding base class, with
the additional suffix of \_EXT.

Generating can take a few moments, so don't worry if it isn't instant.
Once it is complete you will see a detailed log of successes, warnings
and errors. If you have done everything right this should be all green.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image11.jpeg" title="VirtualBox" width="500" %}   

 

3\. Activate the new Service

In order to be able to use your service you will first have to activate
it. This means adding it to a list of services which can be reached
through Gateway. Because I am using integrated Gateway this is done in
the same system, however for those of you using a separate Gateway
instance, login to you Gateway server first.

Navigate to transaction /IWFND/MAINT\_SERVICE

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image12.jpeg" title="VirtualBox" width="500" %}   

 

Click "Add Service"

Because my Gateway instance is on the same box as my ERP, I enter
"LOCAL" as the system alias. To learn more about setting up system
aliases you can read this document. 

Enter the name of your service and hit enter.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image13.jpeg" title="VirtualBox" width="500" %}   

 

Select your service from the list.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image14.jpeg" title="VirtualBox" width="500" %}   

 

Hit save and navigate back to the Activate and Maintain Services screen.
Scroll down the list of services to find your one.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image15.jpeg" title="VirtualBox" width="500" %}   

 

4\. Test the Service from a browser

We now want to see if our service is accessible from the web, to
validate whether we have set it up correctly.

Click the "Call Browser" button in the bottom left section of the
screen.

If your call was successful you will see something like this:

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image16.jpeg" title="VirtualBox" width="500" %}   

 

Now we can modify the URL so the suffix is
"\$metadata?sap-ds-debug=true"

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image17.jpeg" title="VirtualBox" width="500" %}   

 

Here we can see our Carrier entity, with its properties, and the fact
that there is an entity set associated with this entity.

If we change the URL again to end in "/Carriers?sap-ds-debug=true" we
will see this error message:

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image18.jpeg" title="VirtualBox" width="500" %}   

 

This is because we haven't yet told our service how to read Carrier
information from the SCARR database table.

5\. Add ABAP to populate the Carriers

Navigate back to the Gateway Service Builder, SEGW.

Expand Service Implementation \> Carriers

Right click on "Get EntitySet (Query)" and select "Go to ABAP Workbench"

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image19.png" title="VirtualBox" width="500" %}

 

Click the tick on the warning message

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image20.png" title="VirtualBox" width="500" %}

 

This then takes you into the ABAP Class where you will need to build
your implementation.

Click on "display object list"

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image21.jpeg" title="VirtualBox" width="500" %}   

Find the CARRIERS\_GET)ENTITYSET method of the left hand side, right
click on it, and chose "Redefine".

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image22.png" title="VirtualBox" width="500" %}

 

Enter this code into the newly redefined method:

select \* from scarr into corresponding fields of table et\_entityset.

Then hit activate.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image23.jpeg" title="VirtualBox" width="500" %}   

 

6\. Test the Service from a browser 

Return to your browser and refresh the page.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image24.jpeg" title="VirtualBox" width="500" %}   

 

As you can see we now get a stream of information back, detailing all
the Carriers stored in the SCARR table in ERP.

7\. Add another entity and link the two together

In this section we will create another entity set for Flights, and
create a link between carriers and flights.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image25.jpeg" title="VirtualBox" width="500" %}   

 

One airline carrier has many flights, but each flight has only one
airline carrier.

7.1 Create an entity "Flight" with corresponding entity set from table
SFLIGHT

Repeat the steps above to create a new entity Flight, and corresponding
entity set Flights. This time, instead of basing it on the structure
SCARR we will use the structure SFLIGHT.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image26.png" title="VirtualBox" width="500" %}

 

Don't forget to hit Generate when you're done!

7.2 Add ABAP to populate the Flights

Navigate to the ABAP workbench for the Flight entity.

Find the method FLIGHTS\_GET\_ENTITYSET, and redefine it.

Enter ABAP to select everything from the flights table.

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image27.jpeg" title="VirtualBox" width="500" %}   

 

Activate the code.

7.3 Test the Service from a Browser, to ensure the Flights entity has
been added

Use the suffix "Flights?sap-ds-debug=true" on your URL to get back all
the flights

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image28.jpeg" title="VirtualBox" width="500" %}   

 

8\. Create a navigation from Carrier to Flights

We want to be able to go from a specific carrier, to all the flights
that carrier has. To do this we need to create a navigation in the
service builder.

First we create a new Association:

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image29.jpeg" title="VirtualBox" width="500" %}   

 

Then we create the navigation:

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image30.jpeg" title="VirtualBox" width="500" %}   

 

Generate.

Now we need to modify the ABAP so that when we ask for a list of flights
for a specific carrier we don't get back all the flights, we just get
back those relevant to us.

Navigate to the ABAP method FLIGHTS\_GET\_ENTITYSET.

Change the code to read:

data: ls\_key like line of it\_key\_tab.

  read table it\_key\_tab into ls\_key with key name = \'Carrid\'.

\*  If there is a carrier ID specified, use it to select only those for
the specific carrier

  if sy-subrc eq 0.

    select \* from sflight into corresponding fields of table
et\_entityset

      where carrid = ls\_key-value.

\*    If there is no carrier specified, read all the flights

  else.

    select \* from sflight into corresponding fields of table
et\_entityset.

  endif.

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image31.jpeg" title="VirtualBox" width="500" %}   

IT\_KEY\_TAB is importing parameter of the method which will hold all
keys which have been passed in. In our example it will contain a carrier
ID f the navigation is from Carrier, because "Carrid" if the key of the
Carrier entity.

9\. Test the Service from a browser

Use the URL suffix "Carriers(\'AA\')/Flights?sap-ds-debug=true" to test
whether your implementation has worked. If it has the list of results
will only show flights where the carrier ID is "AA".

 

{% include lightbox.html src="/img/2017-11-19-building-your-first-simple-gateway-service/image32.jpeg" title="VirtualBox" width="500" %}   

 

10.  Wrap up

Congratulations, you've just built your first, easy Gateway service.
Obviously this post does not go into the complexities of building a
Gateway service, but I hope it has given you an idea about where to
begin.
