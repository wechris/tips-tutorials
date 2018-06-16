---
layout: post
title: OpenSSL Certificate Authority Part2 
description: Create the intermediate CA.
date: 2018-06-16 15:06:28+0100
date_modified: 2018-06-16 15:06:28+0100
categories: [openssl,ssl,tls,security,certificate]
tags:
  - openssl
  - ssl
  - tls
  - security
  - certificate
author: wechris
image:
  path: /img/2018-06-16-OpenSSL-Certificate-Authority-Part2/postpreview.jpg
  width: 629
  height: 600
---
# OpenSSL Certificate Authority Part2

## Create the intermediate CA with OpenSSL

{% include lightbox.html src="/img/2018-06-16-OpenSSL-Certificate-Authority-Part2/image001.jpg" title="VirtualBox" width="500" %}

### Requirements

OpenSSL is a free and open-source cryptographic library that provides several command-line tools for handling digital certificates. 

### Prepare the directory

The root CA files are kept in /home/wechris/ca. Choose a different directory (/home/wechris/ca/intermediate) to store the intermediate CA files.

```
mkdir /home/wechris/ca/intermediate
```

Create the same directory structure used for the root CA files. It’s convenient to also create a csr directory to hold certificate signing requests.

```
cd /home/wechris/ca/intermediate
mkdir certs crl csr newcerts private
chmod 700 private
touch index.txt
echo 1000 > serial
```

Add a crlnumber file to the intermediate CA directory tree. crlnumber is used to keep track of certificate revocation lists.

```
echo 1000 > /home/wechris/ca/intermediate/crlnumber
```

Copy the intermediate CA configuration file from the Appendix to /home/wechris/ca/intermediate/openssl.cnf. Five options have been changed compared to the root CA configuration file:

```
[ CA_default ]
dir             = /home/wechris/ca/intermediate
private_key     = $dir/private/intermediate.key.pem
certificate     = $dir/certs/intermediate.cert.pem
crl             = $dir/crl/intermediate.crl.pem
policy          = policy_loose
```

### Create the intermediate key

Create the intermediate key (intermediate.key.pem). Encrypt the intermediate key with AES 256-bit encryption and a strong password.

```
cd /home/wechris/ca
openssl genrsa -aes256 \
      -out intermediate/private/intermediate.key.pem 4096

Enter pass phrase for intermediate.key.pem: secretpassword
Verifying - Enter pass phrase for intermediate.key.pem: secretpassword

chmod 400 intermediate/private/intermediate.key.pem
```

### Create the intermediate certificate

Use the intermediate key to create a certificate signing request (CSR). The details should generally match the root CA. The **Common Name**, however, must be different.

_Make sure you specify the intermediate CA configuration file (intermediate/openssl.cnf)._

```
cd /home/wechris/ca
openssl req -config intermediate/openssl.cnf -new -sha256 \
      -key intermediate/private/intermediate.key.pem \
      -out intermediate/csr/intermediate.csr.pem

Enter pass phrase for intermediate.key.pem: secretpassword
You are about to be asked to enter information that will be incorporated
into your certificate request.
-----
Country Name (2 letter code) [XX]:DE
State or Province Name []:Bavaria
Locality Name []: Bmaberg
Organization Name []: Development
Organizational Unit Name []:IchAG
Common Name []:wechris Intermediate CA
Email Address []:
```

To create an intermediate certificate, use the root CA with the ```v3_intermediate_ca``` extension to sign the intermediate CSR. The intermediate certificate should be valid for a shorter period than the root certificate. Ten years would be reasonable.

_This time, specify the root CA configuration file (/home/wechris/ca/openssl.cnf)._

```
cd /home/wechris/ca
openssl ca -config openssl.cnf -extensions v3_intermediate_ca \
      -days 3650 -notext -md sha256 \
      -in intermediate/csr/intermediate.csr.pem \
      -out intermediate/certs/intermediate.cert.pem

Enter pass phrase for ca.key.pem: secretpassword
Sign the certificate? [y/n]: y

chmod 444 intermediate/certs/intermediate.cert.pem
```

The ```index.txt``` file is where the OpenSSL ca tool stores the certificate database. Do not delete or edit this file by hand. It should now contain a line that refers to the intermediate certificate.

```
V 250408122707Z 1000 unknown ... /CN=Alice Ltd Intermediate CA
```

### Verify the intermediate certificate

As we did for the root certificate, check that the details of the intermediate certificate are correct.

```
openssl x509 -noout -text \
      -in intermediate/certs/intermediate.cert.pem
```

Verify the intermediate certificate against the root certificate. An OK indicates that the chain of trust is intact.

```
openssl verify -CAfile certs/ca.cert.pem \
      intermediate/certs/intermediate.cert.pem

intermediate.cert.pem: OK
```

### Create the certificate chain file

When an application (eg, a web browser) tries to verify a certificate signed by the intermediate CA, it must also verify the intermediate certificate against the root certificate. To complete the chain of trust, create a CA certificate chain to present to the application.

To create the CA certificate chain, concatenate the intermediate and root certificates together. We will use this file later to verify certificates signed by the intermediate CA.

```
cat intermediate/certs/intermediate.cert.pem \
      certs/ca.cert.pem > intermediate/certs/ca-chain.cert.pem
chmod 444 intermediate/certs/ca-chain.cert.pem
```

_The certificate chain file must include the root certificate because no client application knows about it yet. A better option, particularly if you’re administrating an intranet, is to install your root certificate on every client that needs to connect. In that case, the chain file need only contain your intermediate certificate._

References:

https://jamielinux.com/docs/openssl-certificate-authority/index.html

https://raymii.org/s/tutorials/OpenSSL_command_line_Root_and_Intermediate_CA_including_OCSP_CRL%20and_revocation.html