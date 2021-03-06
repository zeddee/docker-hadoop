# Hadoop Docker (customized)

Based on [https://github.com/big-data-europe/docker-hadoop](https://github.com/big-data-europe/docker-hadoop)

## Supported Hadoop Versions
* 2.7.1 with OpenJDK 7
* 2.7.1 with OpenJDK 8

## Contents

- [Quick Start](#quick-start)
- [Publish to `localhost`](#publish-to-localhost)
- [Configure Environment Variables](#configure-environment-variables)

## Quick Start

To deploy an example HDFS cluster, run:
```
  docker-compose up
```

`docker-compose` creates a docker network that can be found by running `docker network list`, e.g. `dockerhadoop_default`.

Run `docker network inpect` on the network (e.g. `dockerhadoop_default`) to find the IP the hadoop interfaces are published on. Access these interfaces with the following URLs:

* Namenode: http://<dockerhadoop_IP_address>:50070/dfshealth.html#tab-overview
* History server: http://<dockerhadoop_IP_address>:8188/applicationhistory
* Datanode: http://<dockerhadoop_IP_address>:50075/
* Nodemanager: http://<dockerhadoop_IP_address>:8042/node
* Resource manager: http://<dockerhadoop_IP_address>:8088/

## Publish to `localhost`

For local testing, or when you need to publish to `localhost` (such as when provisioning with Vagrant, or running on macOS):

`docker-compose -f docker-compose-local.yml up`

This publishes hadoop interfaces on the following URLS:

* Namenode: localhost:50070/dfshealth.html#tab-overview
* History server: localhost:58188/applicationhistory
* Nodemanager: localhost:58042/node
* Resource manager: localhost:58088/

## Configure Environment Variables

The configuration parameters can be specified in the hadoop.env file or as environmental variables for specific services (e.g. namenode, datanode etc.):
```
  CORE_CONF_fs_defaultFS=hdfs://namenode:8020
```

CORE_CONF corresponds to core-site.xml. fs_defaultFS=hdfs://namenode:8020 will be transformed into:
```
  <property><name>fs.defaultFS</name><value>hdfs://namenode:8020</value></property>
```
To define dash inside a configuration parameter, use double underscore, such as YARN_CONF_yarn_log___aggregation___enable=true (yarn-site.xml):
```
  <property><name>yarn.log-aggregation-enable</name><value>true</value></property>
```

The available configurations are:
* /etc/hadoop/core-site.xml CORE_CONF
* /etc/hadoop/hdfs-site.xml HDFS_CONF
* /etc/hadoop/yarn-site.xml YARN_CONF
* /etc/hadoop/httpfs-site.xml HTTPFS_CONF
* /etc/hadoop/kms-site.xml KMS_CONF

If you need to extend some other configuration file, refer to base/entrypoint.sh bash script.