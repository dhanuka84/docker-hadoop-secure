# Apache Hadoop 2.7.1 Docker image with Kerberos enabled

[![Docker Pulls](https://img.shields.io/docker/pulls/knappek/hadoop-secure.svg)](https://hub.docker.com/r/knappek/hadoop-secure)
[![Software License](https://img.shields.io/badge/license-MIT-brightgreen.svg?style=flat-square)](LICENSE.md)

This project is a fork from [sequenceiq hadoop-docker](https://github.com/sequenceiq/hadoop-docker) 
and extends it with Kerberos enabled. With docker-compose 2 containers get
created, one with MIT KDC installed and one with a single node kerberized
Hadoop cluster.

The Docker image is also available on [Docker Hub](https://hub.docker.com/r/knappek/hadoop-secure/).

Versions
--------

* JDK8 
* Hadoop 2.7.1
* Maven 3.5.0

Default Environment Variables
-----------------------------

| Name | Value | Description |
| ---- | ----  | ---- |
| `KRB_REALM` | `EXAMPLE.COM` | The Kerberos Realm, more information [here](https://web.mit.edu/kerberos/krb5-1.12/doc/admin/conf_files/krb5_conf.html#) |
| `DOMAIN_REALM` | `example.com` | The Kerberos Domain Realm, more information [here](https://web.mit.edu/kerberos/krb5-1.12/doc/admin/conf_files/krb5_conf.html#) |
| `KERBEROS_ADMIN` | `admin/admin` | The KDC admin user |
| `KERBEROS_ADMIN_PASSWORD` | `admin` | The KDC admin password |
| `KERBEROS_ROOT_USER_PASSWORD` | `password` | The password of the Kerberos principal `root` which maps to the OS root user |

You can simply define these variables in the `docker-compose.yml`.


Run image
---------


Clone the [Github project](https://github.com/dhanuka84/docker-hadoop-secure) and run

```
docker-compose -f docker-kdc.yml up -d

docker exec -it <container-name> /bin/bash

kadmin.local:  addprinc -randkey nn/hadoop.docker.com@EXAMPLE.COM

kadmin.local:  addprinc -randkey dn/hadoop.docker.com@EXAMPLE.COM

kadmin.local:  addprinc -randkey spnego/hadoop.docker.com@EXAMPLE.COM

kadmin.local:  addprinc -randkey jhs/hadoop.docker.com@EXAMPLE.COM

kadmin.local:  addprinc -randkey yarn/hadoop.docker.com@EXAMPLE.COM

kadmin.local:  addprinc -randkey rm/hadoop.docker.com@EXAMPLE.COM

kadmin.local:  addprinc -randkey nm/hadoop.docker.com@EXAMPLE.COM


kadmin.local:  addprinc -randkey hive/hadoop.docker.com@EXAMPLE.COM

kadmin.local:  addprinc -randkey hive/hive-metastore@EXAMPLE.COM

--------------------------------------


kadmin.local:  ktadd -k /opt/nn.service.keytab  nn/hadoop.docker.com

kadmin.local:  ktadd -k /opt/dn.service.keytab  dn/hadoop.docker.com

kadmin.local:  ktadd -k /opt/spnego.service.keytab  spnego/hadoop.docker.com

kadmin.local:  ktadd -k /opt/jhs.service.keytab  jhs/hadoop.docker.com

kadmin.local:  ktadd -k /opt/yarn.service.keytab  yarn/hadoop.docker.com

kadmin.local:  ktadd -k /opt/nm.service.keytab  nm/hadoop.docker.com

kadmin.local:  ktadd -k /opt/rm.service.keytab  rm/hadoop.docker.com


kadmin.local:  ktadd -k /opt/hive.keytab  hive/hive-metastore

------------------------------------------

kinit nn/hadoop.docker.com@EXAMPLE.COM -k -t /etc/security/keytabs/nn.service.keytab 



docker-compose -f docker-hdfs.yml up -d

docker exec -it <container-name> /bin/bash

kinit

docker-compose -f docker-hive.yml up -d
```

