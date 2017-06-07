# Pseudo-Distributed Local Install

Each HBase daemon (HMaster, HRegionServer, ZooKeeper) runs as a separate process
> In standalone mode, all daemons run in one JVM process/instance.


```shell
# 1. Stop HBase if it is running.

# 2. Configure
vim hbase-site.xml
# add
# direct HBase to run in distributed mode, with one JVM instance per daemon.
<property>
    <name>hbase.cluster.distributed</name>
    <value>true</value>
</property>
# change
# local fs to hdfs
<property>
    <name>hbase.rootdir</name>
    <value>hdfs://localhost:9000/hbase</value>
</property>

# 3. Start HBase
bin/start-hbase.sh

# 4. Check the HBase directory in HDFS.
bin/hadoop fs -ls /hbase

# 5. Create a table and populate it with data

# 6. Start and stop a backup HBase Master (HMaster) server.
bin/local-master-backup.sh start 2 3 5
cat /tmp/hbase-testuser-1-master.pid | xargs kill -9

# 7. Start and stop additional RegionServers
bin/local-regionservers.sh start 2 3 4 5
bin/local-regionservers.sh stop 3

# 8. Stop HBase
bin/stop-hbase.sh
```
