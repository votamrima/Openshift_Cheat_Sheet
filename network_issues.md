# Network Issues

## Exposing applications using TLS:

### Create certificates:

**Generate a self signed root certificate with a key**

````
openssl req -x509 -newkey rsa:2048 -keyout CA-key.key -out CA.pem
````

**Generate a certificate request with a key**

````
openssl genrsa -out certificate.key 2048
openssl req -new -key certificate.key -out certificate.csr
````

**Sign a certificate request using CA certificate above**

````
openssl x509 -req -in certificate.csr -CA CA.pem -CAkey CA-key.key -CAcreateserial -out certificate.crt
````

**Upload TLS certificate to Openshift**

````
oc create secret tls tls-certs --cert certificate.crt --key certificate.key -n my-project
````

**Mount secret into the directory**

````
oc set volumes deployment/my-application --add --type secret --secret-name tls-certs --mount-path /opt/app/application/certs
````

**Create a passthrough route**

````
oc create route passthrough --service my-service --hostname my-application.<url>
````

