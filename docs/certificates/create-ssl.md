# Create SSl certificate

1. Create a key: 

	```
	openssl genrsa -out example.develop.key 2048
	```
2. Create a CSR:

	```
	openssl req -new -key example.develop.key -out example.develop.csr

	```
	
	You’ll get all the same questions as you did above and, again, your answers don’t matter. In fact, they matter even less because you won’t be looking at this certificate in a list next to others.

	```
	You are about to be asked to enter information that will be incorporated
	into your certificate request.
	What you are about to enter is what is called a Distinguished Name or a DN.
	There are quite a few fields but you can leave some blank
	For some fields there will be a default value,
	If you enter '.', the field will be left blank.
	-----
	Country Name (2 letter code) []:NL
	State or Province Name (full name) [Some-State]:Gelderland
	Locality Name (eg, city) []:Doetinchem
	Organization Name (eg, company) []:LauLaman Apps
	Organizational Unit Name (eg, section) []:
	Common Name (e.g. server FQDN or YOUR name) []:LauLaman Apps
	Email Address []:email@example.com
	
	Please enter the following 'extra' attributes
	to be sent with your certificate request
	A challenge password []:
	An optional company name []:
	```
	
3. Create an certificate extension config file, which is used to define the Subject Alternative Name (SAN) for the certificate. In our case, we’ll create a configuration file called `example.develop.ext` containing the following text:

	```
	authorityKeyIdentifier=keyid,issuer
	basicConstraints=CA:FALSE
	keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
	subjectAltName = @alt_names
	
	[alt_names]
	DNS.1 = example.develop
	```

4. Now we run the command to create the certificate: using our CSR, the CA private key, the CA certificate, and the config file:
	
	```
	openssl x509 -req -in example.develop.csr -CA myCA.pem -CAkey myCA.key \
	-CAcreateserial -out example.develop.crt -days 825 -sha256 -extfile example.develop.ext
	```
	
	We now have four files: example.develop.key (the private key), example.develop.csr (the certificate signing request, or csr file), example.develop.ext (the certificate extension config file) and example.develop.crt (the signed certificate). 

5. You can no remove the .csr and .ext. since rhey are no longer needed

6. add the certificate and key to the configuration of traefik:
	
	```
	# configuration/certificates.yaml
	tls:
	  certificates:
	    [...]
	
	    - certFile: /certificates/other.cert
	      keyFile: /certificates/other.key
	```
