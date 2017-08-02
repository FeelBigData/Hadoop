# Setup

## Setting up a Single Node Cluster

### Required Software

```shell
# Download too slow, you should do it ahead.
wget http://download.oracle.com/otn-pub/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz?AuthParam=1493362856_bd90d1a1564edfb2808021923145faeb
tar -xvf jdk-*-*.tar.gz

# edit PATH
vim .bashrc
PATH=$PATH:/path/to/jdk/bin

#  
source .bashrc

java -version
```

### Installing Software

- ssh
- rsync

### Download Hadoop

```shell
wget http://mirror.bit.edu.cn/apache/hadoop/common/hadoop-2.7.3/hadoop-2.7.3.tar.gz
```

### Prepare to Start the Hadoop Cluster

```shell
# unpack hadoop.tar
gunzip hadoop.tar.gz
tar -xvf hadoop.tar

# edit ./etc/hadoop/hadoop-env.sh
export JAVA_HOME=/path/to/jdk

# try and display the usage doc for the hadoop script
./bin/hadoop

# edit HADOOP_HOME to PATH
vim .bashrc
HADOOP_HOME=/path/to/hadoop/bin
PATH=$PATH:$HADOOP_HOME

#  
source .bashrc

hadoop
```

### Pseudo-Distributed Operation

In a pseudo-distributed mode, each Hadoop daemon runs in a separate Java process.

#### Configuration

```shell
vim etc/hadoop/core-site.xml
```

```xml
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    <property>
</configuration>
```

```shell
vim etc/hadoop/hdfs-site.xml
```

```xml
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
</configuration>
```

#### Setup passphraseless ssh

Check out ssh to the localhost without a passphrase.

```shell
ssh localhost
```

Set ssh to localhost without a passphrase

```shell
ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
chmod 0600 ~/.ssh/authorized_keys
```

#### Run a MapReduce job locally.

##### 1. Format the filesystem

``` shell
bin/hdfs namenode -format
```

##### 2. Start NameNode daemon and DataNoe daemon

```
sbin/start-dfs.sh
```
> daemon log >> $HADOOP\_HOME/logs

##### 3. Browse the web interface for the NameNode; by default it is available at:
- NameNode - http://localhost:50070/

##### 4. Make the HDFS directories required to execute MapReduce jobs:

```shell
bin/hdfs dfs -mkdir /user
bin/hdfs dfs -mkdir /user/<username>
```

##### 5. Copy the input files into the distributed filesystem:

```shell
bin/hdfs dfs -put etc/hadoop input
```

##### 6. Run example

```shell
bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar grep input output 'dfs[a-z.]+'
```

##### 7. Examine the output files

1. Copy from distributed filesystem to local filesystem

```shell
bin/hdfs dfs -get output output
cat output/*
```

2. View the output files on the distributed filesystem

```shell
bin/hdfs -cat output/*
```

##### 8. Stop daemons

```shell
sbin/stop-dfs.sh
```

#### YARN on a Single Node

Run a MapReduce job on YARN in a pseudo-distributed mode by setting a few parameters and running ResourceManager daemon and NodeManager daemon in addition.

##### 1. Configure parameters

``` shell
vim etc/hadoop/mapred-site.xml
```

``` xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
</configuration>
```

``` shell
vim etc/hadoop/yarn-site.xml
```

``` xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
</configuration>
```

##### 2. Star ResourceManager daemon and NodeManager daemon

```shell
sbin/start-yarn.sh
```

##### 3. Browse the web interface for the ResourceMnager; by default it is aavailable at:

- ResoruceManager - http://localhost:8088/

##### 4. Run a MapReduce job.

##### 5. Stop daemons

```shell
sbin/stop-yarn.sh
```

