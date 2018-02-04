---
layout: post
title: Git client for ABAP 
description: Git client for ABAP on SAP NetWeaver AS ABAP 751 SP02 Developer Edition.
date: 2018-02-04 12:35:13+0100
date_modified: 2018-02-04 12:35:13+0100
categories: [sap,git,github,sap netweaver,abap]
tags:
  - sap
  - git
  - github
  - sap netweaver
  - abap
author: wechris
image:
  path: /img/2018-02-04-Git-client-for-ABAP/image000.jpg
  width: 629
  height: 600
---
# abapGit

abapGit is a git client for ABAP developed in ABAP.

see Repository: [abapgit](https://github.com/larshp/abapGit)

Information: [Readme](https://github.com/larshp/abapGit/blob/master/README.md)

Required: ABAP Version: 702 or higher

Installation Latest build: [zabapgit.abap](https://raw.githubusercontent.com/abapGit/build/master/zabapgit.abap)

[Install guide](http://docs.abapgit.org/guide-install.html)

Copy the ABAP code into new report via <code>SE38</code> or <code>SE80</code>. To update abapGit to a newer version, replace the code in the report with the most recent.

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image001.jpg" title="abapGit" width="500" %}

Currently 53663 lines of code!
After 'Activate' press 'Execute' to run the report:

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image002.jpg" title="abapGit" width="500" %}

The menue item 'Explore' contains some projects using abapGit:
See dotabap.org

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image003.jpg" title="abapGit" width="500" %}

The menue item 'Advanced' contains some useful tools:

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image004.jpg" title="abapGit" width="500" %}

### Prepare to use online repositories

To use the online feature, SSL must be setup. Offline projects will work behind firewalls and without SSL.

To use the online Repos, it is necessery to add the certificates of the Repo hosting site to the SAP trust center (STRUST)

I used Firefox to go to https://github.com and click on the lock icon and then “More Information …” and there “View Certificate”
Switch to the Details Tab and choose each certificates of the tree and export each to a file.

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image006.jpg" title="abapGit" width="500" %}

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image007.jpg" title="abapGit" width="500" %}

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image008.jpg" title="abapGit" width="500" %}

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image009.jpg" title="abapGit" width="500" %}

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image010.jpg" title="abapGit" width="500" %}

### Clone a online Repository

Click the “Clone” link

Enter the url for the github project, eg. https://github.com/se38/zJSON.git along with a package name, eg. ZJSON.
If the package doesn't exits, click 'Create package'.

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image012.jpg" title="abapGit" width="500" %}

Click ok and it will now copy all objects from the git repository into the SAP system

{% include lightbox.html src="/img/2018-02-04-Git-client-for-ABAP/image013.jpg" title="abapGit" width="500" %}