# Create & install Root certificate
1. Create Root CA key: 

	```
	openssl genrsa -des3 -out myCA.key 2048
	```
	OpenSSL will ask for a passphrase, which we recommend not skipping and 	keeping safe. The passphrase will prevent anyone who gets your private key 	from generating a root certificate of their own. The output should look like 	this:
	
	```
	Generating RSA private key, 2048 bit long modulus
	.................................................................+++
	.....................................+++
	e is 65537 (0x10001)
	Enter pass phrase for myCA.key:
	Verifying - Enter pass phrase for myCA.key:
	```



2. Generate a root certificate

	```
	openssl req -x509 -new -nodes -key myCA.key -sha256 -days 1825 -out myCA.pem
	```
	You will be prompted for the passphrase of the private key you just chose and a bunch of questions. The answers to those questions aren’t that important. They show up when looking at the certificate, which you will almost never do. I suggest making the Common Name something that you’ll recognize as your root certificate in a list of other certificates. That’s really the only thing that matters.
	
	```

	Enter pass phrase for myCA.key:
	You are about to be asked to enter information that will be incorporated
	into your certificate request.
	What you are about to enter is what is called a Distinguished Name or a DN.
	There are quite a few fields but you can leave some blank
	For some fields there will be a default value,
	If you enter '.', the field will be left blank.
	-----
	Country Name (2 letter code) []:EU
	State or Province Name (full name) []: Gelderland
	Locality Name (eg, city) []:Doetinchem
	Organization Name (eg, company) []:LauLaman Apps
	Organizational Unit Name (eg, section) []:
	Common Name (e.g. server FQDN or YOUR name) []:LauLaman Apps
	Email Address []:email@example.com

	```
	You should now have two files: myCA.key (your private key) and myCA.pem (your root certificate).

3. Add the Root Certificate to macOS Keychain:
	
	```
	sudo security add-trusted-cert -d -r trustRoot -k "/Library/Keychains/System.keychain" myCA.pem
	```
4.	🎉 Congratulations, you’re now a CA. Sort of.