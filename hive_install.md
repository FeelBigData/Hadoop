# Hive Installation and Configuration

## Install Hive from a Stable Release

```shell
# download
wget http://mirror.bit.edu.cn/apache/hive/stable-2/apache-hive-2.1.1-bin.tar.gz

# unpack tarball
tar -xzvf hive-x.y.z.tar.gz

# set ENV HIVE_HOME
vim .bashrc
HIVE_HOME=/path/to/hive/
PATH=PATH:$HIVE_HOME/bin

source .bashrc
```

## Running Hive

Use HDFS commands to create /tmp and /user/hive/warehouse(aka hive.metastore.warehouse.dir)
and set them chmod g+w before user can create a table in Hive.

```shell
$HADOOP_HOME/bin/hadoop fs -mkdir     /tmp
$HADOOP_HOME/bin/hadoop fs -chmod g+w /tmp

cp $HIVE_HOME/conf/hive-default.xml.template $HIVE_HOME/conf/hive-site.xml
vim hive-site.xml
hive.metastore.warehouse.dir `value` -> /path/to/your/warehouse
hive.metastore.schema.vertification `value` -> false

$HADOOP_HOME/bin/hadoop fs -mkdir     /path/to/your/warehouse
$HADOOP_HOME/bin/hadoop fs -chmod g+w /path/to/your/warehouse

```

### Running Hive CLI

```shell
$HIVE_HOME/bin/hive
```
