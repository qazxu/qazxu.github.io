---
layout: mypost
title: windows环境连接hadoop分布式下的hbase数据库
categories: [数据库]
---

项目中的数据源
```
#hbase

spring.datasource.hbase.driver-class-name=org.apache.phoenix.jdbc.PhoenixDriver
spring.datasource.hbase.jdbc-url=jdbc:phoenix:xxx.xxx.xx.xxx:xxxx
spring.datasource.hbase.data-username=
spring.datasource.hbase.data-password=
```

出现的问题:
winutils.exe 文件未找到

```
07-05 13:45:20.068 [main] WARN  org.apache.hadoop.util.Shell - Did not find winutils.exe: {}
java.io.FileNotFoundException: java.io.FileNotFoundException: HADOOP_HOME and hadoop.home.dir are unset. -see https://wiki.apache.org/hadoop/WindowsProblems
	at org.apache.hadoop.util.Shell.fileNotFoundException(Shell.java:534)
	at org.apache.hadoop.util.Shell.getHadoopHomeDir(Shell.java:555)
	at org.apache.hadoop.util.Shell.getQualifiedBin(Shell.java:578)
	at org.apache.hadoop.util.Shell.<clinit>(Shell.java:675)
	at org.apache.hadoop.util.StringUtils.<clinit>(StringUtils.java:78)
```
解决：下载hadoop的tar包，以及winutils.exe（包含shell等文件）,注意两个必须是同一个版本下

hadoop:
https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-3.0.2/?tdsourcetag=s_pcqq_aiomsg

winutil.exe:
https://github.com/steveloughran/winutils/tree/master/hadoop-3.0.0/bin?tdsourcetag=s_pcqq_aiomsg

然后用下载的winutil.exe等文件替换到hadoop解压后的bin目录中。


在本地hosts文件中添加服务器数据库hosts映射，可到服务器etc\hosts中查看，如：xxx.xxx.xx.xxx hbase01

C:\Windows\System32\drivers\etc\hosts