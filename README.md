# Requirements

- Python 3.11

```
pip install git+https://github.com/michimussato/SSLGeneration.git@packaging
```

```
TS=$(date +%y-%m-%d_%H-%M-%S)
mkdir -p thinkbox-ssl-gen_${TS}
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

# Usage

First generate the CA file

```
python ssl_gen.py --ca --cert-org Thinkbox --cert-ou IT
```
This will dump the ca keys in a folder aptly named 'keys'

Generate the server certificate
```
python ssl_gen.py --server --cert-name <cert_name>
```

This will create a server.crt and server.key file in the 'keys' folder.

Generate the client certificate
```
python ssl_gen.py --client --cert-name <cert_name>
```

Generate a pfx certificate
```
python ssl_gen.py --pfx --cert-name <cert_name>
```

