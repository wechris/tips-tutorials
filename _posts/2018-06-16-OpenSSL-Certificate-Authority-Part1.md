---
layout: post
title: OpenSSL Certificate Authority Part1 
description: Create the root CA.
date: 2018-06-16 12:04:54+0100
date_modified: 2018-06-16 12:04:54+0100
categories: [openssl,ssl,tls,security,certificate]
tags:
  - openssl
  - ssl
  - tls
  - security
  - certificate
author: wechris
image:
  path: /img/2018-06-16-OpenSSL-Certificate-Authority-Part1/postpreview.jpg
  width: 629
  height: 600
---
# OpenSSL Certificate Authority Part1

{% include lightbox.html src="/img/2018-06-16-OpenSSL-Certificate-Authority-Part1/image001.jpg" title="VirtualBox" width="500" %}

## How to setup your own CA with OpenSSL

For educational and testing reasons I created my own CA.

1. Create the Root CA
2. Create an intermediate CA
3. Create a server certificate
4. Create a user certificate

### Requirements

OpenSSL is a free and open-source cryptographic library that provides several command-line tools for handling digital certificates. 

## Create the Root CA
A certificate authority (CA) is an entity that signs digital certificates. 
Typically, the root CA does not sign server or client certificates directly. 
The root CA is only ever used to create one or more intermediate CAs, which are trusted by the root CA to sign certificates on their behalf. 
This is best practice. It allows the root key to be kept offline and unused as much as possible, as any compromise of the root key is disastrous.

### Prepare the directory structure

Create a directory just like (/home/wechris/ca) to store all keys and certificates.

```shell
mkdir /home/wechris/ca

cd /home/wechris/ca

mkdir certs crl newcerts private
chmod 700 private
touch index.txt
echo 1000 > serial
```
The index.txt and serial files act as a flat file database to keep track of signed certificates.

### Prepare the configuration file

You must create a configuration file for OpenSSL to use.
Copy the root CA configuration file from the repository to ```/home/wechris/ca/openssl.cnf```.

The ```[ ca ]``` section is mandatory. Here we tell OpenSSL to use the options from the ```[ CA_default ]``` section.

The ```[ CA_default ]``` section contains a range of defaults. Make sure you declare the directory you chose earlier ('/root/wechris/ca').

The ```[policy_strict]```will be applied for all root CA signatures, as the root CA is only being used to create intermediate CAs.

The ```[policy_loose]```will be applied for all intermediate CA signatures, as the intermediate CA is signing server and client certificates that may come from a variety of third-parties.

The options from the ```[ req ]``` section are applied when creating certificates or certificate signing requests.

The ```[ req_distinguished_name ]``` section declares the information normally required in a certificate signing request. I specified some defaults.

#### The next few sections are extensions that can be applied when signing certificates.

The ```[v3_ca]``` extension are applied, when we create the root certificate.

The ```[v3_ca_intermediate]``` extension are apllied, when we create the intermediate certificate. pathlen:0 ensures that there can be no further certificate authorities below the intermediate CA.

The ```[usr_cert extension]```are apllied, when signing client certificates, such as those used for remote user authentication.

The ```[server_cert extension]``` are applied, when signing server certificates, such as those used for web servers.

_For example, passing the -extensions v3_ca command-line argument will apply the options set in [ v3_ca ]._

The ```crl_ext``` extension is automatically applied when creating certificate revocation lists.

The ```ocsp``` extension are applied, when signing the Online Certificate Status Protocol (OCSP) certificate.

_Whenever you use the req tool, you must specify a configuration file to use with the -config option, otherwise OpenSSL will default to  ```/etc/ssl/openssl.cnf```._


```shell
[ ca ]
default_ca = CA_default

[ CA_default ]
# Directory and file locations.
dir               = /home/wechris/ca
certs             = $dir/certs
crl_dir           = $dir/crl
new_certs_dir     = $dir/newcerts
database          = $dir/index.txt
serial            = $dir/serial
RANDFILE          = $dir/private/.rand

# The root key and root certificate.
private_key       = $dir/private/ca.key.pem
certificate       = $dir/certs/ca.cert.pem

# For certificate revocation lists.
crlnumber         = $dir/crlnumber
crl               = $dir/crl/ca.crl.pem
crl_extensions    = crl_ext
default_crl_days  = 30

# SHA-1 is deprecated, so use SHA-2 instead.
default_md        = sha256

name_opt          = ca_default
cert_opt          = ca_default
default_days      = 730
preserve          = no
policy            = policy_strict

[ policy_strict ]
# The root CA should only sign intermediate certificates that match.
countryName             = match
stateOrProvinceName     = match
organizationName        = match
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional

[ policy_loose ]
# Allow the intermediate CA to sign a more diverse range of certificates.
countryName             = optional
stateOrProvinceName     = optional
localityName            = optional
organizationName        = optional
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional

[ req ]
# Options for the `req` tool.
default_bits        = 2048
distinguished_name  = req_distinguished_name
string_mask         = utf8only

# SHA-1 is deprecated, so use SHA-2 instead.
default_md          = sha256

# Extension to add when the -x509 option is used.
x509_extensions     = v3_ca

[ req_distinguished_name ]
#
countryName                     = Country Name (2 letter code)
stateOrProvinceName             = State or Province Name
localityName                    = Locality Name
0.organizationName              = Organization Name
organizationalUnitName          = Organizational Unit Name
commonName                      = Common Name
emailAddress                    = Email Address

# Optionally, specify some defaults.
countryName_default             = DE
stateOrProvinceName_default     = Bavaria
localityName_default            = Bamberg
0.organizationName_default      = Development
organizationalUnitName_default  = IchAG
emailAddress_default            =

[ v3_ca ]
# Extensions for a typical CA (`man x509v3_config`).
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true
keyUsage = critical, digitalSignature, cRLSign, keyCertSign

[ v3_intermediate_ca ]
# Extensions for a typical intermediate CA (`man x509v3_config`).
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid:always,issuer
basicConstraints = critical, CA:true, pathlen:0
keyUsage = critical, digitalSignature, cRLSign, keyCertSign

[ usr_cert ]
# Extensions for client certificates (`man x509v3_config`).
basicConstraints = CA:FALSE
nsCertType = client, email
nsComment = "OpenSSL Generated Client Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
keyUsage = critical, nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth, emailProtection

[ server_cert ]
# Extensions for server certificates (`man x509v3_config`).
basicConstraints = CA:FALSE
nsCertType = server
nsComment = "OpenSSL Generated Server Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer:always
keyUsage = critical, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth

[ crl_ext ]
# Extension for CRLs (`man x509v3_config`).
authorityKeyIdentifier=keyid:always

[ ocsp ]
# Extension for OCSP signing certificates (`man ocsp`).
basicConstraints = CA:FALSE
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
keyUsage = critical, digitalSignature
extendedKeyUsage = critical, OCSPSigning
```

### Create the root key

Create the root key (ca.key.pem) and keep it absolutely secure. Anyone in possession of the root key can issue trusted certificates. Encrypt the root key with AES 256-bit encryption and a strong password.

_Use 4096 bits for all root and intermediate certificate authority keys. You’ll still be able to sign server and client certificates of a shorter length._

```
cd /home/wechris/ca
openssl genrsa -aes256 -out private/ca.key.pem 4096

Enter pass phrase for ca.key.pem: secretpassword
Verifying - Enter pass phrase for ca.key.pem: secretpassword

chmod 400 private/ca.key.pem
```

### Create the root certificate CA

Use the root key (ca.key.pem) to create a root certificate (ca.cert.pem). Give the root certificate a long expiry date, such as twenty years. Once the root certificate expires, all certificates signed by the CA become invalid.

```
 cd /home/wechris/ca
 openssl req -config openssl.cnf \
      -key private/ca.key.pem \
      -new -x509 -days 7300 -sha256 -extensions v3_ca \
      -out certs/ca.cert.pem

Enter pass phrase for ca.key.pem: secretpassword
You are about to be asked to enter information that will be incorporated
into your certificate request.
-----
Country Name (2 letter code) [XX]:DE
State or Province Name []:Bavaria
Locality Name []: Bamberg
Organization Name []: Development
Organizational Unit Name []:IchAG
Common Name []:wechris ROOT CA
Email Address []:

chmod 444 certs/ca.cert.pem
```

### Verify the root certificate
```
openssl x509 -noout -text -in certs/ca.cert.pem
```
The output shows:

+ the `Signature` Algorithm used
+ the `dates` of certificate Validity
+ the `Public-Key` bit length
+ the `Issuer`, which is the entity that signed the certificate
+ the `Subject`, which refers to the certificate itself

The `Issuer` and `Subject` are identical as the certificate is self-signed. Note that all root certificates are self-signed.

```bash
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 12861279914933409287 (0xb27c71f0ce78d607)
    Signature Algorithm: sha256WithRSAEncryption
        Issuer: C=DE, ST=Bavaria, L=Bamberg, O=IchAG, OU=Development, CN=wechris CA
        Validity
            Not Before: Mar  7 17:10:04 2018 GMT
            Not After : Mar  2 17:10:04 2038 GMT
        Subject: C=DE, ST=Bavaria, L=Bamberg, O=IchAG, OU=Development, CN=wechris CA
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (4096 bit)
```

References:

https://jamielinux.com/docs/openssl-certificate-authority/index.html

https://raymii.org/s/tutorials/OpenSSL_command_line_Root_and_Intermediate_CA_including_OCSP_CRL%20and_revocation.html