
# Basic CRL checking with certutil 
 
https://learn.microsoft.com/en-us/archive/blogs/pki/basic-crl-checking-with-certutil 
 
 ``certutil -f –urlfetch -verify mycertificatefile.cer``

 
 
# Check the CSR and CERT are generated with the same KEY

*PRIVATE KEY*

````
openssl rsa -noout -modulus -in www.myname.com.key | openssl md5
MD5(stdin)= ba699f65a4223cc257ecbd9a4ef75643
````

*CSR*

````
openssl req -noout -modulus -in www.myname.com.csr | openssl md5
MD5(stdin)= ba699f65a4223cc257ecbd9a4ef75643
````

*CER*

````
openssl x509 -noout -modulus -in www.myname.com.cer | openssl md5
MD5(stdin)= ea6a39341b0bd66f0aa848e19099abf8
````
In this case CER are NOT generated with the same KEY



# Create a SAN Cert

`````bash
## Generate SAN CSR
openssl req -new -nodes \
  -out groupit.csr \
  -newkey rsa:2048 \
  -keyout groupit.key \
  -config csr.conf
````

**csr.conf**
````
[ req ]
req_extensions = v3_req
distinguished_name = req_distinguished_name
x509_extensions = usr_cert
prompt = no

[ req_distinguished_name ]
countryName = ES
stateOrProvinceName = Barcelona
localityName = Barcelona
organizationName = ACME Holding 
organizationalUnitName = IT
commonName = groupit.acme.intern
# You might want to adjust values accordingly
# Comments can be included by preceding them with the "#" character

[ v3_req ]
basicConstraints = CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
extendedKeyUsage = serverAuth, clientAuth, codeSigning, emailProtection
subjectAltName = @alt_names


[RequestAttributes]
CertificateTemplate = "SynlabWebServer"  ; Specify the certificate template

[ usr_cert ]
basicConstraints = CA:FALSE
nsCertType = client, server, email
keyUsage = nonRepudiation, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth, clientAuth, codeSigning, emailProtection
nsComment = "OpenSSL Generated Certificate"
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer

[ alt_names ]
DNS.1 = $req_distinguished_name::commonName
DNS.2 = coreit.acme.intern
#DNS.3 = 
#IP.1 = 10.80.1.3
# You might want to adjust values accordingly
# OpenSSL does not support multiple occurrences of the same field within a section.
# To specify multiple values append a numeric identifier, e.g. "DNS.1", "DNS.2" etc.
~

````

**Check**
````
#ver en el CSR
openssl req -in groupit.synlab.intern.csr -text -noout

#ver el Certificate
openssl x509 -in cert.pem -text -noout
````
