---
title: "spark-default.conf"
toc: true
# header:
#   image: /assets/images/spark/spark_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/f3/Apache_Spark_logo.svg)"
layout: posts
categories:
  - Spark
tags:
  - Spark
toc: true
toc_sticky: true

date: 2023-05-14
last_modified_at: 2023-05-14
---

```yaml
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

# Default system properties included when running spark-submit.
# This is useful for setting default environmental settings.

# Example:
# spark.master                     spark://master:7077
# spark.eventLog.enabled           true
# spark.eventLog.dir               hdfs://namenode:8021/directory
# spark.serializer                 org.apache.spark.serializer.KryoSerializer
# spark.driver.memory              5g
# spark.executor.extraJavaOptions  -XX:+PrintGCDetails -Dkey=value -Dnumbers="one two three"
spark.master                     yarn
spark.eventLog.enabled           true
spark.eventLog.dir               hdfs://clustername/user/spark/log
spark.history.fs.logDirectory    hdfs://clustername/user/spark/log
spark.history.provider           org.apache.spark.deploy.history.FsHistoryProvider
spark.history.ui.port            18088
spark.lineage.enabled            true
spark.sql.hive.metastore.jars    path
spark.sql.hive.metastore.jars.path file:///opt/apps/hive-3.1.2/lib/*
spark.sql.hive.metastore.version 3.1.2
spark.sql.hive.metastore.sharePrefixes org.postgresql
spark.driver.extraClassPath      file:///opt/apps/spark-3.2.3/jars/*
spark.executor.extraClassPath    file:///opt/aps/spark-3.2.3/jars/*
spark.yarn.jars                  local:/opt/apps/spark-3.2.3/jars/*
spark.yarn.historyServer.address=http://FQDN:18088 # history 서버를 반드시 명시해야 yarn에서 바로 spark history 서버로 넘어갈 수 있다.
#spark.extraListeners            com.hortonworks.spark.atlas.SparkAtlasEventTracker
#spark.sql.queryExecutionListeners      com.hortonworks.spark.atlas.SparkAtlasEventTracker
#spark.sql.streaming.streamingQueryListeners    com.hortonworks.spark.atlas.SparkAtlasStreamingQueryEventTracker
#spark.authenticate=false
#spark.driver.log.dfsDir=/user/spark/driverLogs
#spark.driver.log.persistToDfs.enabled=true
#spark.dynamicAllocation.enabled=true
#spark.dynamicAllocation.executorIdleTimeout=60
#spark.dynamicAllocation.minExecutors=0
#spark.dynamicAllocation.schedulerBacklogTimeout=1
#spark.eventLog.enabled=true
#spark.io.encryption.enabled=false
#spark.lineage.enabled=true
#spark.network.crypto.enabled=false
#spark.serializer=org.apache.spark.serializer.KryoSerializer
#spark.shuffle.service.enabled=true
#spark.shuffle.service.port=7337
#spark.ui.enabled=true
#spark.ui.killEnabled=true
#spark.pyspark.driver.python=/usr/bin/python3.6
#spark.pyspark.python=/usr/bin/python3.6
#spark.executor.extraClassPath=/usr/lib/mariadb-java-client-2.7.6.jar:/usr/lib/ojdbc8.jar
#spark.driver.extraClassPath=/usr/lib/mariadb-java-client-2.7.6.jar:/usr/lib/ojdbc8.jar
#spark.master=yarn
#spark.submit.deployMode=client
#spark.eventLog.dir=hdfs://siltronhadoop/user/spark/applicationHistory
#spark.yarn.historyServer.address=http://FQDN:18088
#spark.yarn.jars=local:/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p1000.24102687/lib/spark/jars/*,local:/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p1000.24102687/lib/spark/hive/*
#spark.driver.extraLibraryPath=/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p1000.24102687/lib/hadoop/lib/native
#spark.executor.extraLibraryPath=/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p1000.24102687/lib/hadoop/lib/native
#spark.yarn.am.extraLibraryPath=/opt/cloudera/parcels/CDH-7.1.7-1.cdh7.1.7.p1000.24102687/lib/hadoop/lib/native
#spark.extraListeners=com.hortonworks.spark.atlas.SparkAtlasEventTracker
#spark.sql.queryExecutionListeners=com.hortonworks.spark.atlas.SparkAtlasEventTracker
#spark.sql.streaming.streamingQueryListeners=com.hortonworks.spark.atlas.SparkAtlasStreamingQueryEventTracker
#spark.yarn.config.gatewayPath=/opt/cloudera/parcels
#spark.yarn.config.replacementPath={{HADOOP_COMMON_HOME}}/../../..
#spark.yarn.historyServer.allowTracking=true
#spark.yarn.appMasterEnv.MKL_NUM_THREADS=1
#spark.executorEnv.MKL_NUM_THREADS=1
#spark.yarn.appMasterEnv.OPENBLAS_NUM_THREADS=1
#spark.executorEnv.OPENBLAS_NUM_THREADS=1
#spark.hadoop.fs.s3a.committer.name=directory
#spark.sql.sources.commitProtocolClass=org.apache.spark.internal.io.cloud.PathOutputCommitProtocol
#spark.sql.parquet.output.committer.class=org.apache.spark.internal.io.cloud.BindingParquetOutputCommitter
```
