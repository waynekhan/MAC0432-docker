# sonarqube-latest

To use this service, you must act as both an internal Certificate Authority (CA) and the server, generating your own private keys and certificates as necessary.

1. As the CA, generate its private key/public certificate pair:

```text
$ openssl req -x509 -new -nodes -keyout ca.key -days 3650 -out ca.crt
..+......+.....+...+.+..+.......+.....+++++++++++++++++++++++++++++++++++++++*..+......+...+..............+.......+.........+..+...+++++++++++++++++++++++++++++++++++++++*.+.........+..+............+.+......+.....+...+......+.+...+.....+.+..+.........+.+..+......+.........+.+..+...+......+......+...+.+...+......+..+.......+..+.+............+.....+......+.......+..+.+.....+......+.+..+..................+...+....+........+...+..........+..+.........+.+..+..................+....+....................+....+.....+......+.+...+..+.......+.....+.......++++++
..........+.....+++++++++++++++++++++++++++++++++++++++*....+..+.......+........+.......+......+..+......+.+...+...+++++++++++++++++++++++++++++++++++++++*..+......+....+..+.+...+..+............+..........+...........+.+...................................+.+...+..+....+........+......+......+...............+.+..+...+.......+.....+.+.....+....+...+.....+..........+.........+.....+...+...+...+.........+......+.+.....++++++
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:SG
State or Province Name (full name) [Some-State]:Singapore
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Wayne Enterprises
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:ca.waynekhan.local
Email Address []:
```

NB: `ca.waynekhan.local` is the FQDN of my CA, yours will differ.

2. As the server, generate its private key and certificate signing request:

```text
$ openssl genpkey -algorithm RSA -out server.key
........+...........+..........+++++++++++++++++++++++++++++++++++++++*......+++++++++++++++++++++++++++++++++++++++*.....+....+.....................+...+.....+.......+......+.....+....+...........+.......+..+....+..+...............+...........................+......+......+.+..............+.........+.+........+...+....+...........+.........+.+..+...+....+...........+..........+..+.............+...+.....+..........+.........+.................+.+........+......+.+..+...+.......+...........+..........+.....+.+......+.........+........+......+.+...+........+..................+......+.+.....+....+.....+...+.............+..+.......+...+..+....+...............+...........+...+.......+...+......+...........++++++
.........+...+.......+...+..+....+++++++++++++++++++++++++++++++++++++++*...+......+.....+.......+..+.+........+.+...+......+.....+......+.+.........+...+..+..........+...+.....+...+..........+...+++++++++++++++++++++++++++++++++++++++*......+...................+......+...........+......+...+..........+............+...+...............+.....+.........+.......+++++
$ openssl req -new -key server.key -out server.csr
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:SG
State or Province Name (full name) [Some-State]:Singapore
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Wayne Enterprises
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:sonarqube-latest.waynekhan.local
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

NB: `sonarqube-latest.waynekhan.local` is the FQDN of my server, yours will differ. You must also create `server.ext`, containing the server's certificate extensions; e.g.,

```text
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = sonarqube-latest.waynekhan.local
```

3. Back as the CA, sign the server's certificate signing request using its private key, certificate, and the server's certificate extensions:

```text
$ openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out server.crt -days 365 -sha256 -extfile server.ext
Certificate request self-signature ok
subject=C=SG, ST=Singapore, O=Wayne Enterprises, CN=sonarqube-latest.waynekhan.local
```
