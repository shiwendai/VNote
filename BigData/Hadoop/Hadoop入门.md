# Hadoop入门
## 第 2 章 从Hadoop框架讨论大数据生态
### 2.1 Hadoop是什么
```
1. Hadoop是一个由Apache基金会开发的分布式系统基础架构
2. 主要解决海量数据的**存储**和海量数据的**分析计算**问题
3. 广义上来说，Hadoop通常是指一个更广泛的概念——Hadoop生态圈
```

### 2.3 Hadoop三大发行版本
```
Hadoop 三大发行版本：Apache、Cloudera、Hortonworks
1. Apache版本最原始（最基础）的版本，对于入门学习最好。
2. Cloudera在大型互联网企业中用的较多。
3. Hortonworks文档较好。
```

### 2.4 Hadoop的优势（4高）
```
1. 高可靠性：Hadoop底层维护多个数据副本，所以即使Hadoop某个计算元素或存储出现故障，也不会导致数据的丢失。
2. 高扩展性：在集群间分配任务数据，可方便扩展数以千计的节点
3. 高效性：在MapReduce的思想下，Hadoop是并行工作的，以加快任务处理速度
4. 高容错性：能够自动将失败的任务重新分配
```

### 2.5 Hadoop组成（面试重点）
**Hadoop1.x与Hadoop2.x的区别**    
![](_v_images/_1553175234_23396.png)

### 2.5.1 HDFS架构概述
```
1. NameNode(nn)：存储文件的元数据，如文件名，文件目录结构，文件属性（生成时间，副本数、文件权限），  
以及每个文件的块列表和块所在的DataNode等。
2. DataNode（dn）：在本地文件系统存储文件块（block）数据，以及块数据的校验和。
3. Secondary NameNode（2nn）：用来监控HDFS状态的辅助后天程序，每个一段时间获取HDFS元数据的快照。
```

### 2.5.2 YARN架构概述
![](_v_images/_1553176111_10288.png)

### 2.5.3 MapReduce架构概述
```
MapReduce将计算过程氛围两个阶段：Map和Reduce
1. Map阶段并行处理输入数据
2. Reduce阶段对Map结果进行汇总
```

### 2.6 大数据技术生态体系
![](_v_images/_1553217905_18341.png)
图中涉及的技术名词解释如下：
```
1. Sqoop：Sqoop是一款开源的工具，主要用于在Hadoop、Hive与传统数据库（MySQL）间进行数据的传递，可以将一个  
关系型数据库中的数据导入到Hadoop的HDFS中，也可以将HDFS的数据导入到关系型数据库中。
```
```
2. Flume：Flume是Cloudera提供的一个高可用的，高可靠的分布式海量日志采集、聚合和传输的系统，Flume支持在日志  
系统中定制各类数据发送方，用于手机数据；同时，Flume提供对数据进行简单处理，并写到各种数据接收方的能力。
```
```
3. Kafka：kafka是一种高吞吐量的分布式发布订阅消息系统，有如下特性：
    1. 通过O(1)的磁盘数据结构提供消息的持久化，这种结构对于即使数以TB的消息存储也能够保持长时间的稳定性能。  
    2. 高吞吐量：即使是非常普通的硬件Kafka也可以支持每秒数百万的消息。
    3. 支持通过Kafka服务器和消费机集群来分区消息。
    4. 支持Hadoop并行数据加载。
```
```
4. Storm：Storm用于“连续计算”，对数据流做连续查询，在计算时就将结果以流的形式输出给用户。
```
```
5. Spark：Spark是当前最流行的开源大数据内存计算框架。可以基于Hadoop上存储的大数据进行计算。
```
```
6. Oozie：Oozie是一个管理Hdoop作业（job）的工作流程调度管理系统。
```
```
7. Hbase：是一个分布式的、面向列的开源数据库。HBase不同于一般的关系数据库，它是一个适合于非结构化数据存储的数据库。
```
```
8. Hive：Hive是基于Hadoop的一个数据仓库工具，可以将结构化的数据文件映射为一张数据库表，并提供简单的SQL查询功能，  
可以将SQL语句转换为MapReduce任务进行运行。 其优点是学习成本低，可以通过类SQL语句快速实现简单的MapReduce统计，  
不必开发专门的MapReduce应用，十分适合数据仓库的统计分析。
```
```
9. Mahout：Apache Mahout是个可扩展的机器学习和数据挖掘库。
```
```
10. ZooKeeper：Zookeeper是Google的Chubby一个开源的实现。它是一个针对大型分布式系统的可靠协调系统，提供的功能包括：  
配置维护、名字服务、 分布式同步、组服务等。ZooKeeper的目标就是封装好复杂易出错的关键服务，将简单易用的接口和性能高效、  
功能稳定的系统提供给用户。
```


## 第 3 章 Hadoop目录结构
### 3.1 Hadoop目录结构
![](_v_images/_1553219282_235.png)
```
1. bin目录：存放对Hadoop相关服务（HDFS、YARN）进行操作的脚本
2. etc目录：Hadoop的配置文件目录，存放Hadoop的配置文件
3. lib目录：存放Hadoop的本地库（对数据进行压缩解压缩功能）
4. sbin目录：存放启动或停止Hadoop相关的脚本
5. share目录：存放Hadoop的依赖jar包、文档、和官方案例
```

## 第4章 Hadoop运行模式
### 4.3 完全分布式运行模式（开发重点）
#### 4.3.2 编写集群分发脚本xsync
* **scp (secure copy) 安全拷贝**
```
1. scp定义：
    scp可以实现服务器与服务器之间的数据拷贝。
```
```
2. 基本语法
    scp        -r        $pdir/$fname                $user@hadoop$host:$pdir/$fname
    命令    递归    要拷贝的文件路径/名称    目的用户   @    主机    ：  目的路径/名称
```
```
3. 案例实操
    a. 在hadoop101上，将hadoop101中/opt/module目录下的软件拷贝到hadoop102上。
        scp    -r    /opt/module    root@hadoop102:/opt/module
    b. 在hadoop103上，将hadoop101服务器上的/opt/module目录下的软件拷贝到hadoop103上。
        scp    -r    /opt/module    root@hadoop103:/opt/module
        
    注意：拷贝过来的/opt/module目录，别忘了在hadoop102、hadoop103、hadoop104上修改所有文件的，  
         所有者和所有者组。sudo chown atguigu:atguigu -R /opt/module
         
    c. 将hadoop101中/etc/profile文件拷贝到hadoop103的/etc/profile上。
         scp    /etc/profile    root@hadoop103:/etc/profile

    注意：拷贝过来的配置文件别忘了source一下/etc/profile。
```



* **rsync 远程同步工具**  
```
rsync主要用于备份和镜像。具有速度快、避免复制相同内容和支持符号链接的优点。
rsyn 和 scp 区别：用rsync做文件的复制要比scp的速度快，rsync只对差异文件做更新。scp只把所有文件都复制过去。
```

1. 基本语法
```
    rsync        -rvl        $pdir/$fname            $user@hadoop$host : $pdir/$fname  
    命令    选项参数      要拷贝文件路径/名称    目的用户@主机：目的路径/名称  

    选项参数说明
    |    选项    |    功能    |
    | --- | --- |
    |    -r     |    递归    |    
    |    -v    |    显示复制过程   |
    |    -l    |    拷贝符号链接    |
```

```
2. 案例实操
    把hadoop101机器上的/opt/software目录同步到hadoop102服务器的root用户下的/opt/目录
    rsync    -rvl    /opt/software/root@hadoop102:/opt/software
```

* **xsync集群分发脚本**
```
    1. 需求：循环复制文件到所有节点的相同目录下
```
```
    2. 需求分析：
        a. rsync命令原始拷贝：
            rsync    -rvl    /opt/module    root@hadoop103:/opt
        b. 期望脚本：xsync   要同步的文件名称
        c. 说明：在/home/atguigu/bin这个目录下存放的脚本，atguigu用户可以在系统任何地方直接执行。
         注意：如果将xsync放到/home/atguigu/bin目录下仍然不能实现全局使用，可以将xsync移动到/usr/local/bin目录下。
```


    3. 脚本实现
        a. 在/home/atguigu目录下创建bin目录，并在bin目录下xsync创建文件，文件内容如下：
```
    [atguigu@hadoop102 ~]$ mkdir bin
    [atguigu@hadoop102 ~]$ cd bin/
    [atguigu@hadoop102 bin]$ touch xsync
    [atguigu@hadoop102 bin]$ vi xsync
```

```
    在该文件中编写如下代码
    #!/bin/bash
    #1 获取输入参数个数，如果没有参数，直接退出
    pcount=$#
    if((pcount==0)); then
    echo no args;
    exit;
    fi

    #2 获取文件名称
    p1=$1
    fname=`basename $p1`
    echo fname=$fname

    #3 获取上级目录到绝对路径
    pdir=`cd -P $(dirname $p1); pwd`
    echo pdir=$pdir

    #4 获取当前用户名称
    user=`whoami`

    #5 循环
    for((host=103; host<105; host++)); do
            echo ------------------- hadoop$host --------------
            rsync -rvl $pdir/$fname $user@hadoop$host:$pdir
    done
```
```
    b. 修改脚本 xsync 具有执行权限
        [atguigu@hadoop102 bin]$ chmod 777 xsync
```
    c. 调用脚本形式：xsync 文件名称
        [atguigu@hadoop102 bin]$ xsync /home/atguigu/bin

#### 4.3.3 集群配置
* **1.集群部署规划**  

|  | hadoop102 | hadoop103 | hadoop104 |
| --- | --- | --- | --- |
| HDFS | NameNode  |    | SecondaryNameNode |
|     | DataNode  | DataNode | DataNode |
| YARN |    | ResourceManager |    |
|     | NodeManager  | NodeManager | NodeManager |

* **2.配置集群**
1. 核心配置文件      
配置core-site.xml
```
<!-- 指定HDFS中NameNode的地址 -->
<property>
    <name>fs.defaultFS</name>
    <value>hdfs://hadoop102:9000</value>
</property>

<!-- 指定Hadoop运行时产生文件的存储目录 -->
<property>
    <name>hadoop.tmp.dir</name>
    <value>/opt/module/hadoop-2.7.2/data/tmp</value>
</property>

```
2.HDFS配置文件  
配置hadoop-env.sh
```
export JAVA_HOME=/opt/module/jdk1.8.0_144
```
配置hdfs-site.xml
```
<property>
		<name>dfs.replication</name>
		<value>3</value>
</property>

<!-- 指定Hadoop辅助名称节点主机配置 -->
<property>
      <name>dfs.namenode.secondary.http-address</name>
      <value>hadoop104:50090</value>
</property>
```
3.YARN配置文件  
配置yarn-env.sh
```
[atguigu@hadoop102 hadoop]$ vi yarn-env.sh
export JAVA_HOME=/opt/module/jdk1.8.0_144
```
配置yarn-site.xml
```
<!-- Reducer获取数据的方式 -->
<property>
		<name>yarn.nodemanager.aux-services</name>
		<value>mapreduce_shuffle</value>
</property>

<!-- 指定YARN的ResourceManager的地址 -->
<property>
		<name>yarn.resourcemanager.hostname</name>
		<value>hadoop103</value>
</property>
```
4. MapReduce配置文件
配置mapred-env.sh
```
export JAVA_HOME=/opt/module/jdk1.8.0_144
```
配置mapred-site.xml
```
<!-- 指定MR运行在Yarn上 -->
<property>
		<name>mapreduce.framework.name</name>
		<value>yarn</value>
</property>
```

* **3.在集群上分发配置好的Hadoop配置文件**
```
[atguigu@hadoop102 hadoop]$ xsync /opt/module/hadoop-2.7.2/
```

* **4.查看文件分发情况**
```
[atguigu@hadoop103 hadoop]$ cat /opt/module/hadoop-2.7.2/etc/hadoop/core-site.xml
```

#### 4.3.4 集群单点启动
* **1. 如果集群是第一次启动，需要格式化NameNode**
```
[atguigu@hadoop102 hadoop-2.7.2]$ hadoop namenode -format
```
* **2. 在hadoop102上启动NameNode**
```
[atguigu@hadoop102 hadoop-2.7.2]$ hadoop-daemon.sh start namenode
[atguigu@hadoop102 hadoop-2.7.2]$ jps
3461 NameNode
```
* **3. 在hadoop102、hadoop103以及hadoop104上分别启动DataNode**
```
[atguigu@hadoop102 hadoop-2.7.2]$ hadoop-daemon.sh start datanode
[atguigu@hadoop102 hadoop-2.7.2]$ jps
3461 NameNode
3608 Jps
3561 DataNode
[atguigu@hadoop103 hadoop-2.7.2]$ hadoop-daemon.sh start datanode
[atguigu@hadoop103 hadoop-2.7.2]$ jps
3190 DataNode
3279 Jps
[atguigu@hadoop104 hadoop-2.7.2]$ hadoop-daemon.sh start datanode
[atguigu@hadoop104 hadoop-2.7.2]$ jps
3237 Jps
3163 DataNode
```

#### 4.3.6 群起集群
* **1. 配置slaves**
```
/opt/module/hadoop-2.7.2/etc/hadoop/slaves
[atguigu@hadoop102 hadoop]$ vi slaves
在该文件中增加如下内容：
hadoop102
hadoop103
hadoop104
同步所有节点配置文件
[atguigu@hadoop102 hadoop]$ xsync slaves
```
* **2. 启动集群**  
（1）如果集群是第一次启动，需要格式化NameNode（**注意格式化之前，一定要先停止上次启动的所有namenode和datanode进程，然后再删除data和log数据**）
```
[atguigu@hadoop102 hadoop-2.7.2]$ bin/hdfs namenode -format
```
（2）启动HDFS
```
[atguigu@hadoop102 hadoop-2.7.2]$ sbin/start-dfs.sh
```
（3）启动YARN
```
[atguigu@hadoop103 hadoop-2.7.2]$ sbin/start-yarn.sh
```
**注意：NameNode和ResourceManger如果不是同一台机器，不能在NameNode上启动 YARN，应该在ResouceManager所在的机器上启动YARN。**  
（4）Web端查看SecondaryNameNode
```
    （a）浏览器中输入：http://hadoop104:50090/status.html
    （b）查看SecondaryNameNode信息，如图2-41所示。
```

#### 4.3.7 集群启动/停止方式总结
* **1.各个服务组件逐一启动/停止**   
（1）分别启动/停止 HDFS
```
hadoop-daemon.sh    start/stop    namenode/datanode/secondarynamenode
```
（2）启动/停止 YARN
```
yarn-daemon.sh    start/stop    resourcemanager/nodemanager
```

* **2.各个模块分开启动/停止（配饰ssh是前提）常用**    
（1）整体启动/停止 HDFS
```
start-dfs.sh    /    stop-dfs.sh
```
（2）整体启动/停止 YARN
```
start-yarn.sh    /    stop-yarn.sh
```




