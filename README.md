# dse-cluster-migration
This project will migrate the data from a table in a source DSE cluster to another table in a target DSE cluster.  In order to run this project you will need a DSE cluster with DSE Analytics (Spark) enabled. 

Clone this repo to one of the spark enabled nodes and build the project

### How to build
To build simply run `mvn clean package` from the root of this project's directory.

After building the project, you should have an executable jar file in the target directory

eg: target/dse-cluster-migration-0.1.jar

### How to run

```
dse spark-submit \
    --conf spark.dse.cluster.migration.fromClusterHost="127.0.0.1" \
    --conf spark.dse.cluster.migration.toClusterHost="127.0.0.1" \
    --conf spark.dse.cluster.migration.fromKeyspace="keyspace1" \
    --conf spark.dse.cluster.migration.toKeyspace="mytestks" \
    --conf spark.dse.cluster.migration.fromTable="old_table" \
    --conf spark.dse.cluster.migration.toTable="new_table" \
    --conf spark.dse.cluster.migration.newtableflag="true" \
    --conf spark.cassandra.input.consistency.level=LOCAL_QUORUM \
    --conf spark.cassandra.output.consistency.level=LOCAL_ONE \
    --conf spark.executor.memory=2G \
    --conf spark.executor.cores=2 \
    --conf spark.cores.max=4 \
    --conf spark.driver.cores=2 \
    --conf spark.driver.memory=2G \
    --conf spark.blockManager.port=38000 \
    --conf spark.broadcast.port=38001 \
    --conf spark.driver.port=38002 \
    --conf spark.executor.port=38003 \
    --conf spark.replClassServer.port=38005 \
    --class phact.MigrateTable target/dse-cluster-migration-0.1.jar
  ```
    
### Spark Properties

Property|Meaning
---|---
spark.dse.cluster.migration.fromClusterHost | Initial contact point of the source cluster
spark.dse.cluster.migration.toClusterHost | Initial contact point of the target cluster
spark.dse.cluster.migration.fromKeyspace | Source Keyspace name
spark.dse.cluster.migration.toKeyspace | Target Keyspace name
spark.dse.cluster.migration.fromTable | Source table name
spark.dse.cluster.migration.toTable | Target table name
spark.dse.cluster.migration.newtableflag | Create the target table? (true or false)
spark.cassandra.input.consistency.level | Consistency level to read from the source cluster
spark.cassandra.output.consistency.level | Consistency level to write to the target cluster
spark.executor.memory |This specifies the amount of memory to use per executor. Because Spark processes data in memory, the more memory an executor has, the less frequent/likely data may spill to disk which incurs slow disk I/O. 
spark.executor.cores |This parameter tells spark how many concurrent tasks that can be run in each executor, and that usually means the number of CPUs to be assigned for each executor.
spark.cores.max |The maximum amount of CPU cores to request for the application from across the cluster (not from each machine). 
spark.driver.cores |Number of cores to use for the driver process, only in cluster mode.
spark.driver.memory |Amount of memory to use for the driver process.
spark.blockManager.port |Port for all the block managers to listen on.  These exist on both the driver and the executors
spark.broadcast.port |Port for the driver's HTTP server to listen on.
spark.driver.port |Port for the drivers to listen on.  This is used for communicating with the executors and the standalone master.
spark.executor.port |Port for the executor to listen on.  This is used for communicating with the driver.
spark.replClassServer.port |Port for the driver's HTTP class server to listen on.  This is only relevant for the Spark shell.



This project is a fork of:  https://github.com/phact/dse-cluster-migration
