<!-- TOC -->
* [Thinkbox SSLGeneration](#thinkbox-sslgeneration)
  * [Requirements](#requirements)
  * [Installation](#installation)
  * [Usage](#usage)
    * [Prep](#prep)
    * [First generate the CA file](#first-generate-the-ca-file)
    * [Generate the server certificate](#generate-the-server-certificate)
    * [Generate the client certificate](#generate-the-client-certificate)
    * [Generate a pfx certificate](#generate-a-pfx-certificate)
    * [Parse the pem file](#parse-the-pem-file)
  * [Help](#help)
<!-- TOC -->

---

# Thinkbox SSLGeneration

## Requirements

- Python 3.11

## Installation

```
pip install git+https://github.com/michimussato/SSLGeneration.git@packaging
```

## Usage

### Prep

```
TS=$(date +%y-%m-%d_%H-%M-%S)
mkdir -p thinkbox-ssl-gen_${TS}
```

### First generate the CA file

```
thinkbox-ssl-gen --ca --cert-org FARM --cert-ou EVIL --keys-dir ./thinkbox-ssl-gen_${TS}
```

```
$ ls -al ./thinkbox-ssl-gen_${TS}
total 16
drwxrwxr-x 2 michaelmus animation 4096 Dec 12 15:23 .
drwxrwxr-x 3 michaelmus animation 4096 Dec 12 15:22 ..
-rw-rw-r-- 1 michaelmus animation 1131 Dec 12 15:23 ca.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:23 ca.key
```

### Generate the server certificate

```
thinkbox-ssl-gen --server --cert-name DB_HOST --alt-name localhost --alt-name 127.0.0.1 --keys-dir ./thinkbox-ssl-gen_${TS}
```

```
$ ls -al ./thinkbox-ssl-gen_${TS}
total 24
drwxrwxr-x 2 michaelmus animation 4096 Dec 12 15:30 .
drwxrwxr-x 3 michaelmus animation 4096 Dec 12 15:22 ..
-rw-rw-r-- 1 michaelmus animation 1131 Dec 12 15:23 ca.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:23 ca.key
-rw-rw-r-- 1 michaelmus animation 1123 Dec 12 15:30 DB_HOST.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:30 DB_HOST.key
```

### Generate the client certificate

```
thinkbox-ssl-gen --client --cert-name deadline-client --keys-dir ./thinkbox-ssl-gen_${TS}
```

```
$ ls -al ./thinkbox-ssl-gen_${TS}
total 32
drwxrwxr-x 2 michaelmus animation 4096 Dec 12 15:31 .
drwxrwxr-x 3 michaelmus animation 4096 Dec 12 15:22 ..
-rw-rw-r-- 1 michaelmus animation 1131 Dec 12 15:23 ca.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:23 ca.key
-rw-rw-r-- 1 michaelmus animation 1123 Dec 12 15:30 DB_HOST.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:30 DB_HOST.key
-rw-rw-r-- 1 michaelmus animation 1090 Dec 12 15:31 deadline-client.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:31 deadline-client.key
```

### Generate a pfx certificate

```
thinkbox-ssl-gen --pfx --cert-name deadline-client --passphrase DB_CERT_PASS --keys-dir ./thinkbox-ssl-gen_${TS}
```

```
$ ls -al ./thinkbox-ssl-gen_${TS}
total 36
drwxrwxr-x 2 michaelmus animation 4096 Dec 12 15:34 .
drwxrwxr-x 3 michaelmus animation 4096 Dec 12 15:22 ..
-rw-rw-r-- 1 michaelmus animation 1131 Dec 12 15:23 ca.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:23 ca.key
-rw-rw-r-- 1 michaelmus animation 1123 Dec 12 15:30 DB_HOST.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:30 DB_HOST.key
-rw-rw-r-- 1 michaelmus animation 1090 Dec 12 15:31 deadline-client.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:31 deadline-client.key
-rw-rw-r-- 1 michaelmus animation 3217 Dec 12 15:34 deadline-client.pfx
```

### Parse the pem file

```
cat ./keys/DB_HOST.crt ./keys/DB_HOST.key > ./keys/mongodb.pem
```

```
$ ls -al ./keys/mongodb.pem
total 40
drwxrwxr-x 2 michaelmus animation 4096 Dec 12 15:35 .
drwxrwxr-x 3 michaelmus animation 4096 Dec 12 15:22 ..
-rw-rw-r-- 1 michaelmus animation 1131 Dec 12 15:23 ca.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:23 ca.key
-rw-rw-r-- 1 michaelmus animation 1123 Dec 12 15:30 DB_HOST.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:30 DB_HOST.key
-rw-rw-r-- 1 michaelmus animation 1090 Dec 12 15:31 deadline-client.crt
-rw-rw-r-- 1 michaelmus animation 1704 Dec 12 15:31 deadline-client.key
-rw-rw-r-- 1 michaelmus animation 3217 Dec 12 15:34 deadline-client.pfx
-rw-rw-r-- 1 michaelmus animation 2827 Dec 12 15:35 mongodb.pem
```

## Help

```
$ thinkbox-ssl-gen --help
usage: thinkbox-ssl-gen [-h] [-v] [-vv]
                        [--ca | --intermediate-ca | --server | --client | --pfx | --revoke | --renew-crl]
                        [--cert-name CERT_NAME] [--cert-org CERT_ORG]
                        [--cert-ou CERT_OU] [--alt-name ALT_NAME]
                        [--keys-dir KEYS_DIR] [--passphrase PASSPHRASE]
                        [--days DAYS]

SSL Certificate Generator

options:
  -h, --help            show this help message and exit
  -v, --verbose         set loglevel to INFO
  -vv, --very-verbose   set loglevel to DEBUG
  --ca                  Generate a CA certificate
  --intermediate-ca     Generate an intermediate ca certificate
  --server              Generate a server certificate
  --client              Generate a client certificate
  --pfx                 Generate a PFX File
  --revoke              Revoke a certificate
  --renew-crl           Renew CRL
  --cert-name CERT_NAME
                        Certificate name (required with --server, --client,
                        and --pfx)
  --cert-org CERT_ORG   Certificate organization (required with --ca)
  --cert-ou CERT_OU     Certificate organizational unit (required with --ca)
  --alt-name ALT_NAME   Subject Alternative Name
  --keys-dir KEYS_DIR   Directory that stores the key files
  --passphrase PASSPHRASE
                        The passphrase with which to encrypt the output file
                        (optional with --pfx)
  --days DAYS           The period of time during which this certificate is
                        intended to be valid
```
