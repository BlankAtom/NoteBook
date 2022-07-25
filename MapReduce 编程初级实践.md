# MapReduce 编程初级实践



## 初期准备

### 执行 Hadoop 自带的 WordCount

>  wordcount 位于 hadoop 路径下 share/hadoop/mapreduce/hadoop-mapreduce-example-3.2.2.jar
>
> （版本号视情况而定

```shell
# 路径视情况而定
hadoop jar /usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-example-3.2.2.jar wordcount /user/wordcount /user/output/result
```



**直接执行应该会爆异常** 

可能的异常有：

**没有`/user/wordcount`路径**

**解决：**新建这个路径

这个路径根据自己的需要进行定义和重命名。

新建的路径指令为`hdfs dfs -mkdir /user/wordcount`



**wordcount路径下没有文件**

**解决：**上传待检索和合并文件到hdfs的、新建的那个路径下

上传指令为：`hdfs dfs -put input.a /user/wordcount`



**爆出异常**

`Error: Could not find or load main class org.apache.hadoop.mapreduce.v2.app.MRAppMaster`

**解决：**增加属性配置

1. 关闭yarn
2. 复制下面内容到yarn-site.xml（路径请酌情更改）

```xml
<property>
	<name>yarn.application.classpath</name>
	<value>        
		/usr/local/hadoop/etc/*,
		/usr/local/hadoop/etc/hadoop/*,
		/usr/local/hadoop/lib/*,
		/usr/local/hadoop/share/hadoop/common/*,
		/usr/local/hadoop/share/hadoop/common/lib/*,
		/usr/local/hadoop/share/hadoop/mapreduce/*,
		/usr/local/hadoop/share/hadoop/mapreduce/lib-examples/*,
		/usr/local/hadoop/share/hadoop/hdfs/*,
		/usr/local/hadoop/share/hadoop/hdfs/lib/*,
		/usr/local/hadoop/share/hadoop/yarn/*,
		/usr/local/hadoop/share/hadoop/yarn/lib/*,
	</value>
</property>
```

3. 复制下面内容到mapred-site.xml（路径请酌情更改）

```xml
<property>
	<name>yarn.application.classpath</name>
	<value>       
		/usr/local/hadoop/etc/*,
		/usr/local/hadoop/etc/hadoop/*,
		/usr/local/hadoop/lib/*,
		/usr/local/hadoop/share/hadoop/common/*,
		/usr/local/hadoop/share/hadoop/common/lib/*,
		/usr/local/hadoop/share/hadoop/mapreduce/*,
		/usr/local/hadoop/share/hadoop/mapreduce/lib-examples/*,
		/usr/local/hadoop/share/hadoop/hdfs/*,
		/usr/local/hadoop/share/hadoop/hdfs/lib/*,
		/usr/local/hadoop/share/hadoop/yarn/*,
		/usr/local/hadoop/share/hadoop/yarn/lib/*,
	</value>
</property>
```

![image-20210427182010442](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210427210336.png)

```
<property>
  <name>yarn.app.mapreduce.am.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
</property>
<property>
  <name>mapreduce.map.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
</property>
<property>
  <name>mapreduce.reduce.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
</property>


hadoop classpath

<property>
  <name>yarn.app.mapreduce.am.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_CLASSPATH}</value>
</property>
<property>
  <name>mapreduce.map.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
</property>
<property>
  <name>mapreduce.reduce.env</name>
  <value>HADOOP_MAPRED_HOME=${HADOOP_HOME}</value>
</property>

```





### 1、前期遇到的问题

1. 