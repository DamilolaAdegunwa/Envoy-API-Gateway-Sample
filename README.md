# Envoy-API-Gateway-Sample

# VS Code Packages:
 - C#
 - Docker
 - Insert GUID
 - PlantUML
 - PlantUML Previewer
 - YAML

--------------------------

# Docker Compose
  - Run: docker-compose up --build
  - Run: docker-compose down

--------------------------

# Envoy:
## Create Certificate:
 We Need OpenSSL for this, unlike dotnet certificates.
 1. Git Bash
 2. Download the binary from Google
 3. Use Ubuntu runing on Windows (WSL)

```
$ openssl
OpenSSL> 

```

## 3. ... 
```
$ explorer.exe .
```

Create a file named: https.config & open file with "vs code"
```
[ req ]
default_bits       = 2048
default_md         = sha256
default_keyfile    = key.pem
prompt             = no
encrypt_key        = no

distinguished_name = req_distinguished_name
req_extensions     = v3_req
x509_extensions    = v3_req

[ req_distinguished_name ]
commonName         = "localhost"

[ v3_req ]
subjectAltName     = DNS:localhost
keyUsage           = critical, digitalSignature, keyEncipherment
extendedKeyUsage   = critical, 1.3.6.1.5.5.7.3.1, 1.3.6.1.5.5.7.3.2 
```
Save file.


```
$ ls
```
>     https.config

```
$ openssl req -config https.config -new -out csr.pem
```
>     Generating a RSA private key
>     ..........................................+++++
>     .........................................................................................................................................+++++
>     writing new private key to 'key.pem'
>     -----

```
$ ls
```
>     csr.pem  https.config  key.pem

```
$ openssl x509 -req -days 365 -extfile https.config -extensions v3_req -in csr.pem -signkey key.pem -out https.crt
```
>     Signature ok
>     subject=CN = localhost
>     Getting Private key

```
$ ls
```
>     csr.pem  https.config  https.crt  key.pem

```
$ explorer.exe .
```

Copy https.crt & key.pem
Paste to can be anywhere, like out dotnet certificate folders, but rather we won't mix them, so paste in project `Envoy` root.


At the end:

Trust the certificate by going to "Manage User Certificates"
Certificates - Current User | Trusted Root Certification Authorities | Certificates | localhost
it is exists... but why?... this one maybe is for Dot Net Core stuff...

We now import the second certificate: ..\Envoy\https.crt