
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
