---
title: "Oozie-site.xml"
toc: true
# header:
#   image: /assets/images/nifi/nifi_logo.svg
#   caption: "Photo credit: [**Wikipedia**](https://upload.wikimedia.org/wikipedia/commons/f/ff/Apache-nifi-logo.svg)"
layout: posts
categories:
  - Oozie
tags:
  - Oozie
  - Trouble Shooting
toc: true
toc_sticky: true

date: 2023-05-14
last_modified_at: 2023-05-14

---

```xml
<?xml version="1.0"?>
<!--
  Licensed to the Apache Software Foundation (ASF) under one
  or more contributor license agreements.  See the NOTICE file
  distributed with this work for additional information
  regarding copyright ownership.  The ASF licenses this file
  to you under the Apache License, Version 2.0 (the
  "License"); you may not use this file except in compliance
  with the License.  You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

  Unless required by applicable law or agreed to in writing, software
  distributed under the License is distributed on an "AS IS" BASIS,
  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  See the License for the specific language governing permissions and
  limitations under the License.
-->
<configuration>
        <property>
                <name>oozie.base.url</name>
                <value>http://FQDN:11000/oozie</value>
        </property>
        <property>
                <name>oozie.service.JPAService.create.db.schema</name>
                <value>true</value>
        </property>
        <property>
                <name>oozie.service.JPAService.jdbc.driver</name>
                <value>org.postgresql.Driver</value>
        </property>
        <property>
                <name>oozie.service.JPAService.jdbc.url</name>
                <value>jdbc:postgresql://FQDN:3306/oozie</value>
        </property>
        <property>
                <name>oozie.service.JPAService.jdbc.username</name>
                <value>oozie</value>
        </property>
        <property>
                <name>oozie.service.JPAService.jdbc.password</name>
                <value>oozie</value>
        </property>
        <property>
                <name>oozie.use.system.libpath</name>
                <value>true</value>
        </property>
        <property>
                <name>oozie.service.HadoopAccessorService.hadoop.configurations</name>
                <value>*=/opt/apps/hadoop-3.2.1/etc/hadoop</value>
        </property>
        <property>
                <name>oozie.service.SparkConfigurationService.spark.configurations</name>
                <value>*=/etc/spark/conf</value>
        </property>
                <property>
                <name>oozie.service.ProxyUserService.proxyuser.user.hosts</name>
                <value>*</value>
        </property>
        <property>
                <name>oozie.service.ProxyUserService.proxyuser.user.groups</name>
                <value>*</value>
        </property>
</configuration>
```
