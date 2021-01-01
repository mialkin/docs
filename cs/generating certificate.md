# Generating self-signed certificate

## Using OpenSSL

Make sure that openssl is installed:

```bash
openssl version
```

Create **https.config** file:

```text
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
commonName             = "localhost"

[ v3_req ]
subjectAltName      = DNS:localhost
keyUsage            = critical, digitalSignature, keyEncipherment
extendedKeyUsage    = critical, 1.3.6.1.5.5.7.3.1, 1.3.6.1.5.5.7.3.2
```

Inside the folder containing **https.config** file run the following command which will generate **key.pem** file containing private key and **csr.pem** file containing certificate request:

```bash
openssl req -config https.config -new -out csr.pem
```

To generate **https.crt** file containing certificate run the following command:

```bash
openssl x509 -req -days 365 -extfile https.config -extensions v3_req -in csr.pem -signkey key.pem -out https.crt
```

## Using .NET CLI

Create a new self-signed certificate file:

```bash
dotnet dev-certs https -ep ${HOME}/.aspnet/https/aspnetapp.pfx -p YOUR_PASSWORD

dotnet dev-certs https --trust
```

Create a **docker-compose.debug.yml** file with the following content:

```yml
version: '3.4'

services:
  webapp:
    image: mcr.microsoft.com/dotnet/core/samples:aspnetapp
    ports:
      - 8000:80
      - 8001:443
    environment:
      - ASPNETCORE_URLS=https://+:443;http://+:80
      - ASPNETCORE_Kestrel__Certificates__Default__Password=YOUR_PASSWORD
      - ASPNETCORE_Kestrel__Certificates__Default__Path=/https/aspnetapp.pfx 
    volumes:
      - ${HOME}/.aspnet/https:/https/
```

Start the container with ASP.NET configured for HTTPS:

```bash
docker-compose -f "docker-compose.debug.yml" up -d
```

[↑ Hosting ASP.NET Core images with Docker over HTTPS](https://docs.microsoft.com/en-us/aspnet/core/security/docker-https)
