# Hadoop

Get hadoop by downloading the binary tar file from apache website.

Run a hadoop example:

> hadoop-2.7.7> bin/hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.7.jar grep input output 'dfs[a-z.]'


## Hadoop Install Modes

### Pseudo-distributed Mode

To generate a password for passwordless ssh:

> ssh-keygen -t dsa -P '' -f ~/.ssh/id_dsa

> cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys

Open the following files, to configure the hadoop components:

* code Softwares/packages/hadoop-2.7.7/etc/hadoop/hadoop-env.sh

```sh
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_181.jdk/Contents/Home
export HADOOP_PREFIX=~/Softwares/packages/hadoop-2.7.7
```

* code Softwares/packages/hadoop-2.7.7/etc/hadoop/core-site.xml

```xml
<property>
  <name>fs.defaultFS</name>
  <value>hdfs://localhost:9000</value>
</property>
```
* code Softwares/packages/hadoop-2.7.7/etc/hadoop/hdfs-site.xml

```xml
<property>
  <name>dfs.replication</name>
  <value>1</value>
</property>
```

* code Softwares/packages/hadoop-2.7.7/etc/hadoop/mapred-site.xml.template

```xml
<property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
```

* code Softwares/packages/hadoop-2.7.7/etc/hadoop/yarn-site.xml

```xml
<property>
  <name>yarn.nodemanager.aux-services</name>
  <value>mapreduce_shuffle</value>
</property>
```

Now format the `namenode` which is the master node of our cluster.

> bin/hdfs namenode -format


Start the master and slave nodes of the distributed file system.

> sbin/start-dfs.sh


## HDFS

* Name Node
* Data Nodes

### Name Node

* Stores location of blocks of a file.
* Stores the location of replicated block.
* Name node crash is recovered using either:
    * metadata files (fsimage and edits file) - requires merging of both files which takes a lot of computation.
    * secondary name node - kept in sync with master name node based on configuration.

### Data Nodes

* Stores block of files.
* Block size is 128MB.
* Any file irrespective of file size is divided using the same block size.