# Standalone HBase

## Get Started with HBase

### Download, Configure and Start HBase in Standalone Mode

```shell
# 1. download
wget http://mirror.bit.edu.cn/apache/hbase/stable/hbase-1.2.6-bin.tar.gz

# 2. extract
tar xzvf hbase-x.y.z-bin.tar.gz
cd hbase-x.y.z.bin/

# 3. set JAVA_HOME environment variable
vim conf/hbase-env.sh
JAVA_HOME=/path/to/jdk

# 4. edit *conf/hbase-site.xml*
vim conf/hbase-site.xml
<configuration>
<property>
    <name>hbase.rootdir</name>
    <value>file:///path/to/hbase</value>
</property>
<property>
    <name>hbase.zookeeper.property.dataDir</name>
    <value>/path/to/zookeeper</value>
</property>
</configuration>

# 5. start HBase
bin/start-hbase.sh*

jps
HMaster

# 6. go to http://localhost:16010 view HBase web ui
```

### Use HBase For the First Time

```shell
# 1. Connect to HBase using `hbase shell`
bin/hbase shell

# 2. Display HBase Shell Help Text
help

# 3. Create a table : specify the table name and the ColumnFamily name.
create 'test', 'cf'

# 4. List Information About Table
list 'test'

# 5. put data into table
put 'test', 'row1', 'cf:a', 'value1'
put 'test', 'row2', 'cf:b', 'value2'
put 'test', 'row3', 'cf:c', 'value3'

# 6. Scan the table for all data at once
scan 'test'

# 7. Get a single row data.
get 'test', 'row1'

# 8. Disable a table
disable 'test'
enable 'test'

# 9. Drop the table
drop 'test'

# 10. quit: HBase is still running in the background
quit
```

### Stop HBase

```shell
bin/stop-hbase.sh

# check HMaster and HRegionServer processess shutdown
jps
```

