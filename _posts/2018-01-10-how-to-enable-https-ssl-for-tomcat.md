---
layout: post
title: How to enable HTTPS/SSL for Tomcat
description: A tutorial to configure SSL settings of Tomcat 8 to support https connection.
date: 2018-01-09 20:53:00+0200
date_modified: 2018-01-09 20:53:00+0200
categories: [tomcat,security,https,ssl]
tags:
  - tomcat
  - security
  - https
  - ssl
author: wechris
image:
  path: /img/2018-01-10-how-to-enable-https-ssl-for-tomcat/tomcat-with-ssl.jpg
  width: 629
  height: 600
---

A tutorial to configure SSL settings of Tomcat 8 to support https connection. :tiger:

sources used:
- [https://tomcat.apache.org/tomcat-8.0-doc/ssl-howto.html](https://tomcat.apache.org/tomcat-8.0-doc/ssl-howto.html)
- [https://blog.eveoh.nl/2014/02/tls-ssl-ciphers-pfs-tomcat/](https://blog.eveoh.nl/2014/02/tls-ssl-ciphers-pfs-tomcat/)

#### Requirements

This tutorial was created with:
- Java Version -> jdk1.8.0_102
- Tomcat version -> apache-tomcat-8.5.15

#### Steps
- [Create a keystore using Java JDK](#create-a-keystore-using-java-jdk)
- [Set keystore in Tomcat](#set-keystore-in-tomcat)
- [Check SSL or https connection](#check-ssl-or-https-connection)

##### Create a keystore using Java JDK

Keystore file is a container for authorization certificates or public key certificates. They are identified by an alias from a trust chain.

Open the `$CATALINA_HOME\conf` directory and create a folder `ssl`. Switch to `$CATALINA_HOME\conf\ssl`.

```
C:.
├───bin
├───conf
│   ├───Catalina
│   ├───ssl
├───lib
├───logs
├───temp
├───webapps
├───work
```
Command: `keytool -genkey -alias tomcat -keyalg RSA -keystore keystore.jks`
```
C:\apache-tomcat-8.5.15> keytool -genkey -alias tomcat -keyalg RSA -keystore keystore.jks
Enter keystore password:
Re-enter new password:
What is your first and last name?
  [Unknown]:  wechris001.vm.local
What is the name of your organizational unit?
  [Unknown]:  development
What is the name of your organization?
  [Unknown]:  cloud
What is the name of your City or Locality?
  [Unknown]:  Bamberg
What is the name of your State or Province?
  [Unknown]:  Bavaria
What is the two-letter country code for this unit?
  [Unknown]:  DE
Is CN=wechris001.vm.local, OU=development, O=cloud, L=Bamberg, ST=Bavaria, C=DE correct?
  [no]:  yes

Enter key password for <tomcat>
        (RETURN if same as keystore password):
Re-enter new password:
```

File **keystore.jks** was created. Check the contents referring to the keystore file (*keystore.jks*) or the keystore alias (*tomcat*).

Command: `keytool -list -keystore keystore.jks`
```terminal
C:\apache-tomcat-8.5.15> keytool -list -keystore keystore.jks
Enter keystore password:

Keystore type: JKS
Keystore provider: SUN

Your keystore contains 1 entry

tomcat, 24.05.2017, PrivateKeyEntry,
Zertifikat-Fingerprint (SHA1): BF:00:46:A7:82:97:A3:24:02:30:56:9A:72:D0:50:E2:AE:6A:3E:22
```
Command: `keytool -list -alias tomcat -keystore keystore.jks`
```terminal
C:\apache-tomcat-8.5.15> keytool -list -alias tomcat -keystore keystore.jks
Enter keystore password:
tomcat, 24.05.2017, PrivateKeyEntry,
Zertifikat-Fingerprint (SHA1): BF:00:46:A7:82:97:A3:24:02:30:56:9A:72:D0:50:E2:AE:6A:3E:22
```

If you need more informations use option '-v'   Verbose-Output

##### Set keystore in Tomcat

In Tomcat directory, edit **server.xml** located in **conf** folder. Find the following expression and uncomment.

```xml
<--
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
    clientAuth="false" sslProtocol="TLS" />
-->
```

Add **keystoreFile** and **keystorePass** parameters.

```xml
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
    clientAuth="false" sslProtocol="TLS"
    keystoreFile="conf/ssl/keystore" keystorePass="geheim" />
```

Modify **chiphers** and **sslProtocol** parameters.
For more information see: 

[https://tomcat.apache.org/tomcat-8.0-doc/config/http.html](https://tomcat.apache.org/tomcat-8.0-doc/config/http.html)

```xml
    <Connector port="8443" protocol="org.apache.coyote.http11.Http11NioProtocol"
    maxThreads="150" SSLEnabled="true" scheme="https" secure="true"
    clientAuth="false" sslProtocol="TLS"
    keystoreFile="conf/ssl/keystore" keystorePass="geheim" 
    sslEnabledProtocols="TLSv1,TLSv1.1,TLSv1.2"
    ciphers="TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256,
	            TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA,
		    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384,
		    TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA,
		    TLS_ECDHE_RSA_WITH_RC4_128_SHA,
		    TLS_RSA_WITH_AES_128_CBC_SHA256,
		    TLS_RSA_WITH_AES_128_CBC_SHA,
		    TLS_RSA_WITH_AES_256_CBC_SHA256,
		    TLS_RSA_WITH_AES_256_CBC_SHA,
		    SSL_RSA_WITH_RC4_128_SHA"
        />
```

##### Check SSL or https connection

Go to [https://localhost:8443](https://localhost:8443) with your browser. The certificate generated was self-signed, so the connection is not secure. You need to trust in your own certificate to continue.

{% include lightbox.html src="/img/2018-01-10-how-to-enable-https-ssl-for-tomcat/connection-not-secure.png" title="Connection is not secure" width="500" %}

In this case, with FireFox, save the certificate in the browser. Now, browser and tomcat share the same certificate and you can access with a SSL or https connection.

{% include lightbox.html src="/img/2018-01-10-how-to-enable-https-ssl-for-tomcat/add-exception.png" title="Add exception" width="500" %}

{% include lightbox.html src="/img/2018-01-10-how-to-enable-https-ssl-for-tomcat/confirm-security-exception.png" title="Confirm security exception" width="500" %}

{% include lightbox.html src="/img/2018-01-10-how-to-enable-https-ssl-for-tomcat/tomcat-with-ssl.png" title="Tomcat with SSL or https connection" width="500" %}