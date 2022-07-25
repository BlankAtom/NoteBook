```shell
FROM centos
MAINTAINER hong<rocaelfaller@gmail.com>

COPY readme /usr/local/readme
COPY core-site.xml    /usr/local/hbase-3.2.2/etc/hadoop/core-site.xml
COPY hdfs-site.xml    /usr/local/hbase-3.2.2/etc/hadoop/hdfs-site.xml
COPY mapred-site.xml  /usr/local/hbase-3.2.2/etc/hadoop/mapred-site.xml
COPY yarn-site.xml    /usr/local/hbase-3.2.2/etc/hadoop/yarn-site.xml
COPY hadoop-env.sh    /usr/local/hbase-3.2.2/etc/hadoop/hadoop-env.sh
COPY workers    /usr/local/hbase-3.2.2/etc/hadoop/workers



ADD jdk-8u281-linux-x64.tar.gz /usr/local/
ADD hadoop-3.2.2.tar.gz /usr/local/
RUN yum -y install vim

ENV MYPATH /usr/local
WORKDIR $MYPATH


ENV JAVA_HOME /usr/local/jdk1.8.0_281
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV HADOOP_HOME /usr/local/hadoop-3.2.2
#ENV ZOOKEEPER_HOME
#ENV HBASE_HOME 
ENV PATH $PATH:$JAVA_HOME/bin:$HADOOP_HOME/bin:$HADOOP_HOME/sbin

EXPOSE 9870 9868 8020

```

