

[toc]

# **HDFS V1 部署**

## **1.**   **单机伪分布式：**

###**1.1**  **环境配置：**

Ubuntu+ Java环境配置：

**![img](file:///C:/Users/wushu/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)**

### **1.2**  **hadoop-1.2.1 配置：**

下载安装 Hadoop-1.2.1

```shell
$ cd ~/Downloads
$ tar -xzf hadoop-1.2.1.tar.gz
$ mv ./hadoop-1.2.1 ~/
$ cd ~/hadoop-1.2.1
$ ./bin/hadoop version
```
安装成功，如图：

**![img](file:///C:/Users/wushu/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)**

修改相应的HDFS环境（core-site.xml 和 hdfs-site.xml），如图：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-14-55.png" alt="Screenshot from 2019-09-19 18-14-55" style="zoom: 80%; float: left" />

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-16-03.png" alt="Screenshot from 2019-09-19 18-16-03" style="zoom: 92%; float: left" />

启动 HDFS 服务：

  ```shell
$ cd ~/hadoop-1.2.1
$ ./bin/hadoop namenode -format   #格式化namenode
  ```
<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-28-43.png" alt="Screenshot from 2019-09-19 18-28-43" style="zoom:75%; float: left" />

启动HDFS服务进程  
启动命令如下：

```shell
$ cd ~/hadoop-1.2.1
$ ./bin/start-dfs.sh
```

遇到了常见问题中的`localhost: Error: JAVA_HOME is not set and could not be found` 的问题:

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-38-05.png" alt="Screenshot from 2019-09-19 18-38-05" style="zoom:80%; float: left" />

于是到hadoop的安装目录修改配置文件`~/hadoop-1.2.1/conf/hadoop-env.sh`，修改为具体的绝对路径，重新启动，用`jps`检测，显示成功：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-43-57.png" alt="Screenshot from 2019-09-19 18-43-57" style="zoom: 67%; float: left" />

### **1.3** **查看HDFS服务信息**

 访问NameNode的Web界面  
  查看HDFS信息—[http://localhost:50070/](http://localhost:50070/),结果如图:

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-44-43.png" alt="Screenshot from 2019-09-19 18-44-43" style="zoom:67%; float: left" />

 `/logs/`下有 HDFS 的相关日志文件：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-44-48.png" alt="Screenshot from 2019-09-19 18-44-48" style="zoom:55%; float: left" />

 

### **1.4** ** **常用的 HDFS Shell 命令**

  ```shell
$ cd ~/hadoop-1.2.1    
$ ./bin/hadoop fs -mkdir -p /user/wushuangyoyo
$ ./bin/hadoop fs -ls .
$ ./bin/hadoop fs -ls /user/dase
$ ./bin/hadoop fs -mkdir input
$ ./bin/hadoop fs -mkdir /input
$ ./bin/hadoop fs -rm -r /input
  ```

新建目录、查看目录内容：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-53-07.png" alt="Screenshot from 2019-09-19 18-53-07" style="zoom:67%;float:left" />

 删除目录：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 18-56-52.png" alt="Screenshot from 2019-09-19 18-56-52" style="zoom:67%;float:left" />

 从本地文件系统向HDFS中上传文件：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\hdfs文件.png" alt="hdfs文件" style="zoom:75%;float:left" />

查看上传到 HDFS 上的 README.txt 的文件内容:

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-13-47.png" alt="Screenshot from 2019-09-19 19-13-47" style="zoom:75%;float:left" />

在上传文件的时候另起一个 terminal ，在上传过程中运行`jps`，查看出现的进程名称：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-15-52.png" alt="Screenshot from 2019-09-19 19-15-52" style="zoom:75%;float:left" />

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-15-52 (2).png" alt="Screenshot from 2019-09-19 19-15-52 (2)"  />

下载测试数据集`test2.8G.text`，保存在`~/Downloads`中  

![Screenshot from 2019-09-19 19-22-11](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-22-11.png)

把文件从HDFS中的一个目录拷贝到HDFS中的另外一个目录：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-23-22.png" alt="Screenshot from 2019-09-19 19-23-22" style="zoom:75%;float:left" />

### **1.5****停止HDFS服务**

```shell
$ cd ~/hadoop-1.2.1
$ ./bin/stop-dfs.sh
```

成功停止：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-23-59.png" alt="Screenshot from 2019-09-19 19-23-59" style="zoom:80%;float:left" />

由于我们之后普遍使用的是 Hadoop-2.9.2，所以此处不再进行 Hadoop-1.2.1 的分布式部署而直接进行 HDFS V2 的部署阶段，分布式部署的部分会在接下来** HDFS V2 部署**的报告中进行。

# **HDFS V2 部署**

## **1.** **单机伪分布式：**

重复步骤不再悉数列举，在这里只呈现不同的地方以及阶段性结果：

### **1.1** **环境部署**

配置环境变量：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-38-30.png" alt="Screenshot from 2019-09-19 19-38-30" style="float:left"  />

![Screenshot from 2019-09-19 19-39-14](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-39-14.png)

### **1.2** **修改 HDFS 配置**

修改 `core-site.xml` 和 `hdfs-site.xml`：

![Screenshot from 2019-09-19 19-40-45](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-40-45.png)

![Screenshot from 2019-09-19 19-40-54](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-40-54.png)



### **1.3** **部署 HDFS**

 格式化 NameNode (仅在首次部署的时候执行, 不可反复执行)

```shell
$ ~/hadoop-2.9.2/bin/hdfs namenode -format
```

格式化成功

![Screenshot from 2019-09-19 19-42-22](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-42-22.png)

启动 HDFS 服务进程

```shell
$ ~/hadoop-2.9.2/sbin/start-dfs.sh
```

![Screenshot from 2019-09-19 19-46-10](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-46-10.png)

至此，HDFS v2 部署完毕。

### **1.4** **常用的 HDFS shell 命令**

目录/文件操作不再赘述，结果如图：

![Screenshot from 2019-09-19 19-55-06](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-55-06.png)

![Screenshot from 2019-09-19 19-55-35](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-55-35.png)

![Screenshot from 2019-09-19 19-56-11](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-56-11.png)

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-56-33 (2) - 副本.png" alt="Screenshot from 2019-09-19 19-56-33 (2) - 副本" style="zoom: 50%; float: left;" />

![Screenshot from 2019-09-19 19-56-33 (2)](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-56-33 (2).png)

![Screenshot from 2019-09-19 19-57-24](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-57-24.png)

### **1.5** **停止 HDFS 服务**

![Screenshot from 2019-09-19 19-59-49](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 19-59-49.png)



## **2.** **分布式：**

和*邹鸿嘉*、*曹云昀*、*熊双宇*、*刘颖凡*组成小组进行分布式的部署：

其中，*熊双宇*（219.228.135.10）为主节点。

### **2.1** **ssh部署：**

创建 **ecnu** 用户：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 20-56-04 - 副本.png" alt="Screenshot from 2019-09-19 20-56-04 - 副本" style="zoom:80%;float:left" />

ssh 部署：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 20-01-16.png" alt="Screenshot from 2019-09-19 20-01-16" style="zoom:75%;float:left" />

免密登录主节点：

![Screenshot from 2019-09-19 21-13-05](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 21-13-05.png)

免密登录其余各个从节点：

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-19 20-01-16\Screenshot from 2019-09-19 21-37-27.png" alt="Screenshot from 2019-09-19 21-37-27" style="zoom:80%;float:left" />

由于*刘颖凡*出现了难以解决的问题，我们暂时形成四个人的分布式结构：

但之后由于 `authorized-keys`的冲突宣告失败。

之后进行了一次抹掉磁盘的重装系统，于是进行第二次尝试。

### **第二次尝试**

这一次我们有*吴双*（219.228.135.148）、*曹云昀*（219.228.135.64）、*刘颖凡*（219.228.135.177），其中*吴双*（219.228.135.148）是主节点。

其他部分和之前一致，实现了互相的免密登录。

但是遇到了如下的问题，

![Screenshot from 2019-10-17 00-54-28](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-10-17 00-39-31\Screenshot from 2019-10-17 00-54-28.png)

后来发现，是因为我们的主节点的 **9000** 通道被主节点的另一个用户的 **Hadoop** 占用了，于是切换到另一个用户关闭 **Hadoop** 重新运行，成功：

![Screenshot from 2019-10-17 18-56-35](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-10-17 00-39-31\Screenshot from 2019-10-17 18-56-35.png)

停止 HDFS 服务，成功：

![Screenshot from 2019-10-17 19-04-48](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-10-17 00-39-31\Screenshot from 2019-10-17 19-04-48.png)

# **MapReduce v2 部署**

## **2** **单机伪分布式部署**

之前做的时候一直报错

![����1](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\����1.png)

后来发现我们不做集中式部署，所以 `core-site.xml` 和 `hdfs-site.xml` 的设置出了问题。~~解决方法：不用解决，我们没有必要做单机集中式。忘了这件事就行。~~

### **2.1** **准备工作**

### **2.2** **修改配置**

  - 修改 `mapred-site.xml` 文件

    ```shell
    mv ~/hadoop-2.9.2/etc/hadoop/mapred-site.xml.template vi ~/hadoop-2.9.2/etc/hadoop/mapred-site.xml
    vi ~/hadoop-2.9.2/etc/hadoop/mapred-site.xml
    ```
    

    在 `<configuration>` 下添加

    ```xml
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    ```
    
    ![����α�ֲ�ʽmapred�޸�](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\����α�ֲ�ʽmapred�޸�.png)
    
    

  - 修改 `yarn-site.xml` 文件

    ```shell
    vi ~/hadoop-2.9.2/etc/hadoop/yarn-site.xml
    ```

    在 `<configuration>` 下添加

    ```xml
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    ```
    
    ![yarn�޸�](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\yarn�޸�.png)
    
    

### **2.3** **启动 MapReduce 服务**

  - 启动 YARN

    ```shell
    ~/hadoop-2.9.2/sbin/start-yarn.sh                               # 启动YARN
    ~/hadoop-2.9.2/sbin/mr-jobhistory-daemon.sh start historyserver # 开启历史服务器, 才能在 web 中查看任务日志
    ```

  - 启动 HDFS

    ```shell
    ~/hadoop-2.9.2/sbin/start-dfs.sh
    ```
    
    ![yarn](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\yarn.png)
    

### **2.4** **查看 MapReduce 服务**

  - 查看服务进程

    ```shell
    >>> jps
    508203 ResourceManager
    506510 SecondaryNameNode
    508096 JobHistoryServer
    508912 Jps
    508357 NodeManager
    506101 NameNode
    506279 DataNode
    ```

    ![yarn - 副本](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\yarn - 副本.png)


  - 访问 http://localhost:50070 查看 HDFS


    ![hdfs](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\hdfs.png)

  - 访问 http://localhost:8088 查看 MapReduce

    ![mapreduce](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\mapreduce.png)

### **2.5** **提交 MapReduce 程序**

  - 提交 Jar 包并查看运行结果

    ```shell
    >>> cd ~/hadoop-2.9.2
    >>> ./bin/hdfs dfs -mkdir -p input/grep                         # 新建输入文件夹
    >>> ./bin/hdfs dfs -put ~/hadoop-2.9.2/etc/hadoop/* input/grep  # 上传输入文件
    >>> ./bin/hdfs dfs -rm -r output/grep                           # 删除输出路径
    >>> ./bin/yarn jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar grep input/grep output/grep 'dfs[a-z.]+'
    >>> ./bin/hdfs dfs -cat output/grep/p*                          # 查看输出文件
    6    dfs.audit.logger
    4    dfs.class
    3    dfs.logger
    3    dfs.server.namenode.
    2    dfs.audit.log.maxfilesize
    ...
    ```
    
    ![mapreduce_����](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\mapreduce_����.png)
![mapreduce���](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\mapreduce���.png)

  - 查看运行过程中的进程

    - 下载测试数据集 `test2.8G.text` 至 `~/Download`
    
- 运行 wordcount 示例, 并且查看系统执行该任务过程中启动的进程:
  
  ```shell
      cd ~/hadoop-2.9.2
      ./bin/hdfs dfs -put ~/Download/test2.8G.text input    # 上传输入文件
      ./bin/hdfs dfs -rm -r output/wordcount                # 删除输出路径
      ./bin/yarn jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar wordcount input/test2.8G.text output/wordcount
  ```
  
  ![Screenshot from 2019-10-21 00-31-02](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\Screenshot from 2019-10-21 00-31-02.png)
  
  ![Screenshot from 2019-10-21 00-32-37](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\Screenshot from 2019-10-21 00-32-37.png)

### **2.6** **停止 MapReduce**

  ```shell
  ~/hadoop-2.9.2/sbin/stop-yarn.sh
  ~/hadoop-2.9.2/sbin/mr-jobhistory-daemon.sh stop historyserver
  ~/hadoop-2.9.2/sbin/stop-dfs.sh
  ```

![stop](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\报错1\stop.png)

## **3 分布式部署**

### **3.2 修改配置**

  - 修改文件`mapred-site.xml` 和 `yarn-site.xml` 并将配置文件拷贝到其他节点:

    ```shell
    scp -r ~/hadoop-2.9.2/etc/hadoop ecnu@192.168.1.12:~/hadoop-2.9.2/etc
    ```
    
    ![mapred_site.xml](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\mapred_site.xml.png)
    
    ![scp_etc](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\scp_etc.png)
    
    ![ecp_etc2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\ecp_etc2.png)

### **3.3 启动 MapReduce**

  在主节点执行以下命令:

  - 启动 YARN

    ```shell
    ~/hadoop-2.9.2/sbin/start-yarn.sh                               # 启动YARN
    ~/hadoop-2.9.2/sbin/mr-jobhistory-daemon.sh start historyserver # 开启历史服务器, 才能在 web 中查看任务日志
    ```

  - 启动 HDFS

    ```shell
    ~/hadoop-2.9.2/sbin/start-dfs.sh
    ```
    
    ![Screenshot from 2019-10-17 00-39-31](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-17 00-39-31.png)

### **3.4 查看 MapReduce 服务**

  - 查看服务进程

    主节点上执行 `jps`

    ```shell
    >>> jps
    608203 ResourceManager
    606510 SecondaryNameNode
    608096 JobHistoryServer
    608912 Jps
    608357 NodeManager
    606101 NameNode
    606279 DataNode
    ```

    从节点上执行 `jps`

    ```shell
    >>> jps
    215060 NodeManager
    214340 DataNode
    215455 Jps
    ```

    若缺少某些进程, 说明启动过程中出现错误, 请在缺少进程的节点上查看服务进程日志, 排查原因.
    日志记录在 `~/hadoop-2.9.2/logs/` 路径下后缀为 `log` 的相关文件

* 网页访问查看效果

    

    访问 http://192.168.1.11:50070 查看 HDFS

    访问 http://192.168.1.11:8088 查看 MapReduce

    访问 http://192.168.1.11:19888 查看历史作业记录

    

    ![Screenshot from 2019-10-21 20-14-40](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 20-14-40.png)
    
    ![Screenshot from 2019-10-21 20-15-07](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 20-15-07.png)

### **3.5 提交 MapReduce 程序**

  - 提交 Jar 包并查看运行结果

    ```shell
    >>> cd ~/hadoop-2.9.2
    >>> ./bin/hdfs dfs -mkdir -p input/grep                        # 新建输入文件夹
    >>> ./bin/hdfs dfs -put ~/hadoop-2.9.2/etc/hadoop/* input/grep # 上传输入文件
    >>> ./bin/hdfs dfs -rm -r output/grep                          # 删除输出路径
    >>> ./bin/yarn jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar grep input/grep output/grep 'dfs[a-z.]+'
    >>> ./bin/hdfs dfs -cat output/grep/p*                         # 查看输出文件
    6    dfs.audit.logger
    4    dfs.class
    3    dfs.logger
    3    dfs.server.namenode.
    2    dfs.audit.log.maxfilesize
    ...
    ```
    
    
    
    ![mapreduce_problem_solved](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\mapreduce_problem_solved.png)
    
    ![mapreduce_verify](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\mapreduce_verify.png)
    
  - 查看运行过程中的进程

    - 下载测试数据集 `test2.8G.text` 至 `~/Download`
      链接: https://pan.baidu.com/s/1xzgp9UCuilqnq1SpovMrRw 密码: 8v7x

    - 运行 wordcount 示例, 并且查看系统执行该任务过程中启动的进程:

      ```shell
      cd ~/hadoop-2.9.2
      ./bin/hdfs dfs -put ~/Download/test2.8G.text input    # 上传输入文件
      ./bin/hdfs dfs -rm -r output/wordcount                # 删除输出路径
      ./bin/yarn jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.2.jar wordcount input/test2.8G.text output/wordcount
      ```

      ![Screenshot from 2019-10-21 21-19-43](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 21-19-43.png)

      ![Screenshot from 2019-10-21 21-25-44](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 21-25-44.png)

# **HDFS 应用编程实践**

##** 1. 前期准备**

安装并配置好IntelliJ IDEA，并且更新maven插件。

- 下载IntelliJ IDEA安装包  
  下载地址：[JetBrains官网](https://www.jetbrains.com/idea/download/#section=linux)  
  可以下载**Ultimate**版本，可以使用*学号邮箱*注册的账号进行激活。

- 安装IntelliJ IDEA  
  运行如下命令，启动安装指导程序，开始图形化界面安装：

  ```shell
  cd ~/Downloads    #切换安装包下载路径
  tar -xzf ideaIU-2018.3.3.tar.gz    #解压安装包
  cd idea-IU-183.5153.38/     #切换解压文件夹
  ./bin/idea.sh    #运行idea安装指导程序
  ```

- 启动IntelliJ IDEA  
  首次运行过程需要激活，输入*注册*过的学号邮箱账号，可继续运行。  
  安装完成后初始界面如下：

  ![](C:/Users/wushu/Documents/temperate/dos/实验/2019-lecture/BigDataSystems_HandsON/HDFS Programming/screenshots/start_interface.png)

- IntelliJ IDEA中是集成了maven插件,也可以配置自己安装的maven版本。  
  配置界面如下：

  ![](C:/Users/wushu/Documents/temperate/dos/实验/2019-lecture/BigDataSystems_HandsON/HDFS Programming/screenshots/maven-configuration.png)

## **2. 使用IntelliJ IDEA编写 `测试HDFS中是否存在一个文件` 应用程序**

#### **添加hadoop依赖**

在pom.xml文件里添加hadoop的依赖包hadoop-common, hadoop-client, hadoop-hdfs 

**注意使用和部署环境统一的版本号**
添加的依赖代码如下：

```xml
<dependencies>
    <!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common -->
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-common</artifactId>
        <version>2.9.2</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-hdfs -->
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-hdfs</artifactId>
        <version>2.9.2</version>
    </dependency>

    <!-- https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-client -->
    <dependency>
        <groupId>org.apache.hadoop</groupId>
        <artifactId>hadoop-client</artifactId>
        <version>2.9.2</version>
    </dependency>
</dependencies>
```

![Screenshot from 2019-09-26 20-03-56](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-26 20-03-56\Screenshot from 2019-09-26 20-03-56.png)

#### **IDE环境编写java程序**

编一个Java应用程序，用来检测HDFS中是否存在一个文件。  
新建HDFSFileIfExist.java类文件，文件代码如下：

```java
import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.fs.FileSystem;
import org.apache.hadoop.fs.Path;
public class HDFSFileIfExist {
    public static void main(String[] args){
        try{

            Configuration conf = new Configuration();

            if (args.length != 2) {
                // 参数设置： hdfs://localhost:9000 ./input/core-site.xml
                // 如果是分布式请填写主节点ip地址或者域名
                System.err.println("Usage: HDFSFileifExist <default file system> <file path>");
                System.exit(2);
            }

            /** 设置需要测试是否存在的文件/目录 */
//            String fileName = "./input/file01.txt";
            String fileName = args[1];

            /** fs.defaultFS - NameNode URI - hdfs://host:port */
//            conf.set("fs.defaultFS", "hdfs://localhost:9000");
            conf.set("fs.defaultFS", args[0]);

            conf.set("fs.hdfs.impl", "org.apache.hadoop.hdfs.DistributedFileSystem");
            FileSystem fs = FileSystem.get(conf);
            if(fs.exists(new Path(fileName))){
                System.out.println("文件/目录存在");
            }else{
                System.out.println("文件/目录不存在");
            }

        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

![Screenshot from 2019-09-26 20-21-27](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-26 20-03-56\Screenshot from 2019-09-26 20-21-27.png)

## **3. 运行 `测试HDFS中是否存在一个文件` 应用程序**

#### **利用 IDE 打包jar文件**

#### **伪分布模式下提交应用程序**

```shell
cp /.../HDFSFileIfExist.jar ~/hadoop-2.9.2/myApp/    #将打包好的可执行jar包拷贝至hadoop安装路径下的myApp/目录下
cd ~/hadoop-2.9.2/
./bin/hadoop jar ./myApp/HDFSFileIfExist.jar hdfs://localhost:9000 ./input/core-site.xml    #使用hadoop命令运行jar包,并且在运行时输入参数
```

![jdajdhka](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\Screenshot from 2019-09-26 20-03-56\jdajdhka.png)

#### **分布式提交应用程序**

```shell
cp /.../HDFSFileIfExist.jar ~/hadoop-2.9.2/myApp/    #将打包好的可执行jar包拷贝至hadoop安装路径下的myApp/目录下
cd ~/hadoop-2.9.2/
./bin/hadoop jar ./myApp/HDFSFileIfExist.jar hdfs://主节点ip:9000 ./input/core-site.xml  #使用hadoop命令运行jar包,并且在运行时输入参数
```

![fileis exist_jar](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\fileis exist_jar.png)

## **5. 附录：练习代码文件**

下面是可供练习的hdfs相关操作代码：

- 列出目录下所有文件：

  ```java
  import org.apache.hadoop.conf.Configuration;
  import org.apache.hadoop.fs.FileStatus;
  import org.apache.hadoop.fs.FileSystem;
  import org.apache.hadoop.fs.Path;
  
  import java.io.IOException;
  
  public class ListHDFSFile {
      public static void main(String[] args) throws IOException {
          Configuration conf = new Configuration();
  
          if (args.length != 2) {
              // 参数设置： hdfs://localhost:9000 ./input
              System.err.println("Usage: ListHDFSFile <default file system> <path>");
              System.exit(2);
          }
  
          /** fs.defaultFS - NameNode URI - hdfs://host:port */
          //conf.set("fs.defaultFS", "hdfs://localhost:9000");
          conf.set("fs.defaultFS", args[0]);
          conf.set("fs.hdfs.impl", "org.apache.hadoop.hdfs.DistributedFileSystem");
          FileSystem fs = FileSystem.get(conf);
  
          /** 设置需要测试是否存在的文件/目录 */
          //String uri = "./input";
          String uri = args[1];
  
          Path path = new Path(uri);
          FileStatus[] status = fs.listStatus(path);
          for (FileStatus fileStatus : status) {
              System.out.println(fileStatus.getPath().toString());
          }
          fs.close();
      }
  }
  ```
  
  运行结果
  
  ![list_jar](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\list_jar.png)
  
  
  
- 写入文件

  ```java
  import org.apache.hadoop.conf.Configuration;
  import org.apache.hadoop.fs.FSDataOutputStream;
  import org.apache.hadoop.fs.FileSystem;
  import org.apache.hadoop.fs.Path;
  
  public class WriteHDFSFile {
      public static void main(String[] args) {
          try {
  
              if (args.length != 3) {
                  //参数设置： hdfs://localhost:9000 ./write-test Hello,Hadoop.
                  System.err.println("Usage: WriteHDFSFile <default file system> <path> <content>");
                  System.exit(2);
              }
  
              Configuration conf = new Configuration();
  
              /** fs.defaultFS - NameNode URI - hdfs://host:port */
              //conf.set("fs.defaultFS", "hdfs://localhost:9000");
              conf.set("fs.defaultFS", args[0]);
              conf.set("fs.hdfs.impl", "org.apache.hadoop.hdfs.DistributedFileSystem");
              FileSystem fs = FileSystem.get(conf);
  
              byte[] buff = args[2].getBytes(); // 要写入的内容
              String filename = args[1]; //要写入的文件名
  
              FSDataOutputStream os = fs.create(new Path(filename));
              os.write(buff,0,buff.length);
              System.out.println("Create:"+ filename);
              os.close();
              fs.close();
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
  }
  ```
  
  ![write_work](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\write_work.png)
  
  ![mapreduce_jar_dos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\write_jar.png)


- 读取文件

  ```java
  import java.io.BufferedReader;
  import java.io.InputStreamReader;
  
  import org.apache.hadoop.conf.Configuration;
  import org.apache.hadoop.fs.FileSystem;
  import org.apache.hadoop.fs.Path;
  import org.apache.hadoop.fs.FSDataInputStream;
  
  public class ReadHDFSFile {
      public static void main(String[] args) {
          try {
              Configuration conf = new Configuration();
  
              if (args.length != 2) {
                  //参数设置： hdfs://localhost:9000 ./input/core-site.xml
                  System.err.println("Usage: ReadHDFSFile <default file system> <path>");
                  System.exit(2);
              }
  
              /** fs.defaultFS - NameNode URI - hdfs://host:port */
              //conf.set("fs.defaultFS", "hdfs://localhost:9000");
              conf.set("fs.defaultFS", args[0]);
              conf.set("fs.hdfs.impl", "org.apache.hadoop.hdfs.DistributedFileSystem");
              FileSystem fs = FileSystem.get(conf);
  
              /** 设置读取文件目录 */
              //String uri = "./input/core-site.xml";
              String uri = args[1];
  
              Path file = new Path(uri);
              FSDataInputStream getIt = fs.open(file);
              BufferedReader d = new BufferedReader(new InputStreamReader(getIt));
              String content = d.readLine(); //读取文件一行
              while (content != null) {
                  System.out.println(content);
                  content = d.readLine();
              }
  
              d.close(); //关闭文件
              fs.close(); //关闭hdfs
          } catch (Exception e) {
              e.printStackTrace();
          }
      }
  }
  ```

  ![read_work](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\read_work.png)
  
  ![read_jar](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\read_jar.png)

##**分布式：**

判断文件是否存在：

![Screenshot from 2019-10-21 21-48-30](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 21-48-30.png)

列出目录下的文件：

![Screenshot from 2019-10-21 21-51-41](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 21-51-41.png)

写入文件：

![Screenshot from 2019-10-21 21-52-46](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 21-52-46.png)

读取文件内容：

![Screenshot from 2019-10-21 21-51-56](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 21-51-56.png)

### **所遇到的问题**

其中，遇到问题:

![problem2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\problem2.png)

原因是导入的`jar`包不全，于是手动导入所有的`hadoop-2.9.2/share/hadoop/common/`文件夹下的jar包和路径，重新运行，成功。

![import_problem2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\import_problem2.png)

![import2_problem2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\import2_problem2.png)

# MapReduce 应用编程实践

##  使用 IDEA 编写 WordCount

在部署的 Hadoop 上运行 WordCount上传输入文件至 HDFS并运行程序（单机伪分布式）。由于不过是重复之前的操作，所以这里直接不在详细阐述各个步骤，直接附上结果：

![Screenshot from 2019-10-21 19-34-54](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\Screenshot from 2019-10-21 19-34-54.png)

![Screenshot from 2019-10-21 19-34-59](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\Screenshot from 2019-10-21 19-34-59.png)

![mapreduce_jar](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\mapreduce_jar.png)

![Screenshot from 2019-10-21 19-34-54](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\Screenshot from 2019-10-21 19-34-54.png)

### **分布式**

查看输出结果

![Screenshot from 2019-10-21 22-03-22](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 22-03-22.png)

### **遇到的问题：**

卡在`map`和 `reduce`之前，Hadoop运行任务时一直卡在: INFO mapreduce.Job: Running job。问题的原因是由于 yarn里面关于内存和虚拟内存的配置项 默认了一个最小选区点。

在 http://hortonworks.com/blog/how-to-plan-and-configure-yarn-in-hdp-2-0/ 有详细解释，

解决方法是：在 `yarn-site.xml` 中加入:

```xml
<property>
    <name>yarn.nodemanager.resource.memory-mb</name>
    <value>20480</value>
</property>
<property>
   <name>yarn.scheduler.minimum-allocation-mb</name>
   <value>2048</value>
</property>
<property>
    <name>yarn.nodemanager.vmem-pmem-ratio</name>
    <value>2.1</value>
</property>

```

这里设置了一个比较低的运行阈值，可以保证大部分的情况下大部分程序可以运行。再次运行，即成功。

# **Hadoop Straming 介绍与实践**

## **Hadoop Streaming 运行**

编译产生可执行 Mapper/Reducer 文件

  ```shell
  cd ~/hadoop-2.9.2/cppTest
  g++ mapper.cpp -o mapper
  g++ reducer.cpp -o reducer
  ```

![mapreducer_cpp](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\mapreducer_cpp.png)

使用 Hadoop Streaming 运行

成功

![mapreduce_work_jar](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\mapreduce_work_jar.png)

![mapreduce_verify](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\mapreduce_verify.png)

## **Hadoop Streaming Shell 示例**

如图编译，并运行。

成功。

<img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\streamingshell_test.png" alt="streamingshell_test" style="zoom:80%;float:left" />

![mapreduce_jar_dos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\weifenbu\mapreduce_jar_dos.png)

分布式运行：

![Screenshot from 2019-10-21 22-09-36](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj1\mapred_site.xml\Screenshot from 2019-10-21 22-09-36.png)





# 4、 **总结**

1. 安装版本最好和教材一致，避免因为版本更新造成命令的变更。
2. 在运行 分布式 的情况下，需要仔细考虑文件到底是放在哪里，是 HDFS 还是本地。
3. 论文要好好看，好多问题的出现其实就是没仔细看论文，没理解本质的含义造成的。