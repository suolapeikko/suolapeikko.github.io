# Certificate Notes and Commands

## Formats

### PEM
Most likely format used by Certificate Authorities. Comes usually with extentions like .pem, .crt, .cer, and .key. PEM files are Base64 encoded ASCII files. File contains "-----BEGIN CERTIFICATE-----" and "-----END CERTIFICATE-----" statements. Contains certificates and private key.

### DER
DER uses binary format for a certificate. It usually comes with .der or .cer extension. Look for BEGIN/END statements in the file to see which format the file really is. Contains certificates and private key.

### PKCS#7/P7B
PKCS#7 or P7B format is usually in Base64 ASCII format and has a file extention of .p7b or .p7c. Contain "-----BEGIN PKCS7-----" and "-----END PKCS7-----" statements. Contains certificates but not private key.

### PKCS#12/PFX
PKCS#12 or PFX format uses binary format and is encryptable. Comes usually with extensions like .pfx and .p12.

## Some Example Commands

### Decode PEM encoded certificate

`$ openssl x509 -in certificate.crt -text -noout`

### Decode DER encoded certificate

`$ openssl x509 -in certificate.crt -inform der -text -noout`

### Create a CSR

`$ openssl req -new -newkey rsa:2048 -nodes -keyout customer.com.key.txt -out customer.com.csr.txt`

### Covert pfx/p12 to text format

`$ openssl pkcs12 -in bundle.customer.com.pfx -out package.pem -nodes`

### Convert key to file from pfx/p12

`$ openssl pkcs12 -in bundle.customer.com.pfx -clcerts -nokeys -out domain.cer`

### Convert certificate to file from pfx/p12

`$ openssl pkcs12 -in bundle.customer.com.pfx -nocerts -nodes  -out domain.key`

### Convert CA certificate to file from pfx/p12

`$ openssl pkcs12 -in domain.pfx -out domain-ca.crt -nodes -nokeys -cacerts`

## Steps to open an existing certificate and building it back to contain certificate chain (eg. for Jamf Pro JSS server SSL certificate)

* Decode pfx or pkcs#12 to text (both are binary formats with same content, pfx is usually used by Microsoft)

`$ openssl pkcs12 -in bundle.customer.com.pfx -out package.pem -nodes`

* Extract key and certificate parts from the file to separate files, eg. mykey.txt and mycert.txt

`$ cp package.pem mykey.txt`
`$ cp package.pem mycert.txt`

Then edit in vi, leaving private key to mykey.txt and certificate to mycert.txt

* Decode certificate to obtain intermediate certificate type (See Issuer -> CN)

`$ openssl x509 -in mycert.txt -text -noout`

* Download intermediate CA file from the web, based on step 3, then copy it to a file, for example geotrust_ssl_ca_g3.txt

* Decode intermediate certificate to obtain root certificate type (See Issuer -> CN)

`$ openssl x509 -in geotrust_ssl_ca_g3.txt -text -noout`

* Download root CA file from the web, based on step 5, then copy it to a file, for example geotrust_global_ca.txt

* Concatenate intermediate and root certificates to a single text file

`$ cat geotrust_ssl_ca_g3.txt geotrust_global_ca.txt > ca_bundle.txt`

* Compile everything back to pfx/p12 again

`$ openssl pkcs12 -export -out bundle.customer.com.p12 -inkey mykey.txt -in mycert.txt -certfile ca_bundle.txt`

* Check the contents of the created p12 certificate bundle

`openssl pkcs12 -info -in bundle.customer.com.p12`
