## 一. Spark 部署

### 1. 单机集中式部署

#### 1.1 安装并运行 Spark 应用程序

- 通过 Spark-Shell 运行应用程序

  - 进入 Spark-Shell

    ```shell
    $ ~/spark-2.4.4/bin/spark-shell --master local
    ```
    
    **遇到问题**：
    
    ![problem_1_file_not_exist](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/problem_1_file_not_exist.png)
    
    问题原因只是因为之前做过一次单机伪分布式的部署，所以这里单机集中式部署会报错，其实只需要把所需要的文件（`hdfs://localhost:9000/tmp/spark_history`）新建一下就可以了
    
    ![problem_file_not_exist_solved](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/problem_file_not_exist_solved.png)
  
- 在 `scala>` 后输入 Scala 代码.
  
  ```scala
  scala> sc.textFile("file:///home/you/spark-2.4.4/RELEASE").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).collect
  ```
  
  执行后应打印出如下结果
  
  ```scala
  scala> res2: Array[(String, Int)] = Array((-Psparkr,1), (-B,1), (Spark,1), (-Pkubernetes,1), (-Pyarn,1), (2.4.4,1), (Build,1), (built,1), (-Pflume,1), (-DzincPort=3036,1), (flags:,1), (-Phive-thriftserver,1), (-Pmesos,1), (for,1), (-Phive,1), (-Pkafka-0-8,1), (2.7.3,1), (-Phadoop-2.7,1), (Hadoop,1))
  ```
  
  如下，运行成功
  
  ![scala_1](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/scala_1.png)
  
- 通过提交 Jar 包运行应用程序

  ```shell
  $ ~/spark-2.4.4/bin/spark-submit \
    --master local \
    --class org.apache.spark.examples.SparkPi \
    ~/spark-2.4.4/examples/jars/spark-examples_2.11-2.4.4.jar
  ```

  运行结果如下图所示:

  ![jar_1](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/jar_1.png)

  ![jar_1_out](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/jar_1_out.png)
  
  在运行过程中另起一个终端执行 `jps` 查看进程.此时只会出现 SparkSubmit 进程, 应用程序运行结束后该进程消失.
  ![jar_1_jps](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/jar_1_jps.png)

### 2. 单机伪分布式部署

#### 2.1 修改配置并启动Spark服务

- 修改 `spark-env.sh` 文件

  ![env_edit](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/env_edit.png)
  
- 修改 `slaves` 文件

  ![slaves](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/slaves.png)
  
- 修改 `spark-defaults.conf` 文件

  ![spark-default-conf](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/spark-default-conf.png)

- 启动命令

  ```shell
  $ ~/spark-2.4.4/sbin/start-all.sh             # 启动 Spark
  $ ~/spark-2.4.4/sbin/start-history-server.sh  # 启动应用日志服务器
  ```
  
  结果如下,可通过`jps`查看进程来检测是否启动成功:
  
  ![spark_pdos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/spark_pdos.png)
  
  也可通过查看 Spark 服务日志
  
  ![logs](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/logs.png)
  
  访问 Spark Web 界面, 可看到 Master 和 Worker: http://localhost:8080
  
  ![web](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/web.png)

#### 2.2 运行 Spark 应用程序

- 通过 Spark-Shell 运行应用程序

  - 准备输入文件

    将本地的 `~/spark-2.4.4/RELEASE` 上传至 HDFS 的 `/user/you/spark_input` 下

  - 进入 Spark-Shell
    
    ```shell
    $ ~/spark-2.4.4/bin/spark-shell --master spark://localhost:7077
    ```
    
    ![scala_shell_localhost](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/scala_shell_localhost.png)
  
- 在 `scala>` 后输入 Scala 代码.
  
  ```scala
  scala> sc.textFile("hdfs://localhost:9000/user/you/spark_input/RELEASE").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).collect
  ```
  
  执行后应打印出如下结果
  
  ```scala
  scala> res2: Array[(String, Int)] = Array((-Psparkr,1), (-B,1), (Spark,1), (-Pkubernetes,1), (-Pyarn,1), (2.4.4,1), (Build,1), (built,1), (-Pflume,1), (-DzincPort=3036,1), (flags:,1), (-Phive-thriftserver,1), (-Pmesos,1), (for,1), (-Phive,1), (-Pkafka-0-8,1), (2.7.3,1), (-Phadoop-2.7,1), (Hadoop,1))
  ```
    如图,运行成功:
  
  ![scala_shell_2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/scala_shell_2.png)
  
- 通过提交 Jar 包运行应用程序
  
- Client 提交模式 (默认), 此模式下 Driver 运行在客户端, 可以在客户端看到应用程序运行过程中的信息
  
  ```shell
  $ ~/spark-2.4.4/bin/spark-submit \
     --deploy-mode client \
     --master spark://localhost:7077 \
     --class org.apache.spark.examples.SparkPi \
     ~/spark-2.4.4/examples/jars/spark-examples_2.11-2.4.4.jar
  ```
  
  在运行过程中另起一个终端执行 `jps` 查看进程.运行结果如下图所示:
  
  ![scala_jar_pdos_jps](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/scala_jar_pdos_jps.png)
  
- Cluster 提交模式, 此模式下 Master 会随机选取一个 Worker 节点启动 Driver, 故在客户端看不到应用程序运行过程中的信息
  
    ```shell
    $ ~/spark-2.4.4/bin/spark-submit \
      --deploy-mode cluster \
      --master spark://localhost:7077 \
      --class org.apache.spark.examples.SparkPi \
      ~/spark-2.4.4/examples/jars/spark-examples_2.11-2.4.4.jar
    ```

    在运行过程中另起一个终端执行 `jps` 查看进程.运行结果如下图所示:
  ![scala_jar_pdos_jps_cluster](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/scala_jar_pdos_jps_cluster.png)

+ 实时查看应用运行情况

  在应用运行过程中 (如进入 Spark-Shell 之后), 访问 http://localhost:4040

  - Client 模式:

    ![web_spark_shell_client](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/web_spark_shell_client.png)

  - Cluster 模式:

    ![web_scala_cluster](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/web_scala_cluster.png)

- 查看 Spark 应用程序日志

  在提交一个应用程序后，在 `~/spark-2.4.4/work` 下会出现应用程序运行日志

  ![ls_logs_pdos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/ls_logs_pdos.png)

- 查看应用历史记录

  在应用运行结束后, 访问 http://localhost:18080
  
  ![web_logs](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/web_logs.png)

#### 2.3 停止服务

- 停止命令

  ```shell
  $ ~/spark-2.4.4/sbin/stop-all.sh              # 停止 Spark
  $ ~/spark-2.4.4/sbin/stop-history-server.sh   # 停止日志服务器
  ```
  ![spark_stop](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-1/spark_stop.png)
  

### 3. 分布式部署

#### 3.1 准备工作&修改配置

- 修改 `spark-env.sh` 文件

  ![spark-env](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\spark-env.png)
  
- 修改 `slaves` 文件, 添加节点 IP

  ```sh
  219.228.135.148
  219.228.135.64
  ```

- 修改 `spark-defaults.conf` 文件

  ![spark-defaults](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\spark-defaults.png)
  
  并在 HDFS 中建立目录 `/tmp/spark_history`
  
  ```shell
   $ ~/hadoop-2.9.2/bin/hdfs dfs -mkdir -p /tmp/spark_history
  ```
  
  遇到问题:
  
  ![problem_1_connection_refused](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_1_connection_refused.png)
  
  产生问题的原因 没有打开是 Spark,在打开 Spark 的过程中遇到另一个问题:
  
  ![problem_1_safe_mode](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_1_safe_mode.png)
  
  解决问题方法如下:
  
  ![problem_1_solved_safemode](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_1_solved_safemode.png)
  
  ![scp_problem_1_solved](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\scp_problem_1_solved.png)


- 将配置好的 Spark 拷贝到其它节点

  ```shell
  $ scp -r ~/spark-2.4.4 ecnu@219.228.135.64:~/
  ```

#### 3.3 启动服务&查看服务信息

- 启动命令 (在主节点上执行),执行 `jps`查看运行进程

  ![problem_1_solved_jps](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_1_solved_jps.png)

- 查看 Spark 服务日志

  - 主节点日志
  
  ![log_1](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\log_1.png)
  
- 访问 Spark Web 界面, 可看到 Master 和 Worker: http://localhost:8080

  ![web_log](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\web_log.png)

#### 3.5 运行 Spark 应用程序

- 通过 Spark-Shell 运行应用程序

  - 准备输入文件

    将本地的 `~/spark-2.4.4/RELEASE` 上传至 HDFS 的 `/user/ecnu/spark_input` 下

    ![put_material](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\put_material.png)

  - 进入 Spark-Shell

    ```
    >>> ~/spark-2.4.4/bin/spark-shell --master spark://219.228.135.148:7077
    ```
    
    遇到问题:
    
    ![problem_2_failedtoconnecttomaster](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_2_failedtoconnecttomaster.png)
    
    hosts 被污染,解决问题方法如下:
    
    ![problem_2_solution](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_2_solution.png)
    
    成功,结果如下:
    
    ![problem_2_solved](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_2_solved.png)
    
  - 在 `scala>` 后输入 Scala 代码.
    此处执行的是统计 `RELEASE` 文件中的单词数量

    ![problem_2_solved_prove](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_2_solved_prove.png)

- 通过提交 Jar 包运行应用程序

  - Client 提交模式 (默认), 此模式下 Driver 运行在客户端, 可以在客户端看到应用程序运行过程中的信息

    ```shell
    >>> ~/spark-2.4.4/bin/spark-submit \
    >>>   --deploy-mode client \
    >>>   --master spark://219.228.135.148:7077 \
    >>>   --class org.apache.spark.examples.SparkPi \
    >>>   ~/spark-2.4.4/examples/jars/spark-examples_2.11-2.4.4.jar
    ```
    
    ![jar_dos_example](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_dos_example.png)
    
    在运行过程中另起一个终端执行 `jps` 查看进程.
    ![jar_prove_pi](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_prove_pi.png)
- Cluster 提交模式, 此模式下 Master 会随机选取一个 Worker 节点启动 Driver, 故在客户端看不到应用程序运行过程中的信息
  
  ```shell
    ~/spark-2.4.4/bin/spark-submit \
    --deploy-mode cluster \
      --master spark://219.228.135.148:7077 \
      --class org.apache.spark.examples.SparkPi \
      ~/spark-2.4.4/examples/jars/spark-examples_2.11-2.4.4.jar
  ```
  
    在运行过程中另起一个终端执行 `jps` 查看进程.
  
   ![dos_cluster](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\dos_cluster.png)

#### 3.6 查看 Spark 程序运行信息

- 实时查看应用运行情况

  在应用运行过程中 (如进入 Spark-Shell 之后), 访问 http://192.168.1.11:4040

  ![web_log_working](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\web_log_working.png)

- 查看 Spark 应用程序日志, 在 `~/spark-2.4.4/work` 下

  在提交一个应用程序后，在各子节点的 `~/spark-2.4.4/work` 下会出现应用程序运行日志

  ![work_logs](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\work_logs.png)

  

#### 3.7 停止服务

- 停止命令 (在主节点上执行)

  ```shell
  ~/spark-2.4.4/sbin/stop-all.sh              # 停止 Spark
  ~/spark-2.4.4/sbin/stop-history-server.sh   # 停止日志服务器
  ```
  



## 二. Spark-RDD-programming

### 1. 编写Spark程序

#### 1.1 新建Maven项目并添加pom依赖


+ 新建maven项目并修改`pom.xml`根据计算机的spark和hadoop-hdfs版本进行相关修改。 

  ![pom](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/pom.png)


####  1.2 IDE环境编写代码并直接运行

  - 按照文档所示编写`java`以及`scala`代码，其中`scala`需要导入`scala-plugin`，配置运行环境如下：

      - Java:

        ![conf](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/conf.png)

      - Scala:

        ![scala_conf](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/scala_conf.png)

    运行结果如图：

    - Java:

      ![program_succeed_pc](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/program_succeed_pc.png)

    - Scala:

      ![scala_result](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/scala_result.png)

#### 1.3 利用IDE打包jar文件并在伪分布模式下提交Spark程序

  - 在Spark的Master节点输入命令:
    
      - Java模式：

        ```shell
        $ ~/spark-2.4.4/bin/spark-submit \
        --master spark://localhost:7077 \
        --class RDDWordCount /home/wushuangyoyo/RDDWordCount/out/artifacts/RDDWordCount/RDDWordCount.jar\
        hdfs://localhost:9000/spark/input hdfs://localhost:9000/spark/output
        ```
        
         **遇到问题**：
        ![problem_exist](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/problem_exist.png)
         看报错，易得问题的原因是`output`文件夹已存在，解决方法即删除该文件夹，重新执行程序，成功运行。结果如下：
      
        ![problem1_exist_solved](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/problem1_exist_solved.png)

        ![problem_exist_verify_1](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/problem_exist_verify_1.png)

        ![problem_exist_solved_verify_2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/problem_exist_solved_verify_2.png)

        ![problem_exist_solved_verify_3](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/problem_exist_solved_verify_3.png)
	
    - Scala模式:
    
       ```shell
       $ ~/spark-2.4.4/bin/spark-submit \
         	--master spark://localhost:7077 \
         	--class RDDWordCount /home/wushuangyoyo/RDDWordCount/out/artifacts/RDDWordCountScala/RDDWordCountScala.jar\
         	hdfs://localhost:9000/spark/input hdfs://localhost:9000/spark/output
       ```
    
       结果如下：
    
       ![scala_jar_pdos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/scala_jar_pdos.png)
    
       ![scala_jar_2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/scala_jar_2.png)
    
       ![scala_jar_3](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-2/scala_jar_3.png)

#### 3.3 分布式模式下提交Spark程序

  - 如下所示(其中20s211为主机名，也可用ip地址)：
  ```
  ~/spark-2.4.4/bin/spark-submit --master spark://219.228.135.148:7077 --class RDDWordCountScala /home/wushuangyoyo/RDDWordCount/out/artifacts/RDDWordCountScala/RDDWordCountScala.jar hdfs://219.228.135.148:9000/spark/input hdfs://219.228.135.148:9000/spark/output
  ```

![dos_jar_spark](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\dos_jar_spark.png)

![status_dos_jar_spark](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\status_dos_jar_spark.png)

## 三. Spark on Yarn 模式部署

### 1 单机伪分布式部署

#### 1.1 修改配置并启动Spark服务

- 修改 `spark-env.sh` 文件, 使 Spark 能够读取 Yarn 配置，结果如下：

  ![spark-yarn-conf](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/spark-yarn-conf.png)
- 启动命令。通过`jps`查看服务信息来确认启动成功：

  ![jps-spark-pdos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/jps-spark-pdos.png)

#### 1.2 运行 Spark 应用程序

- 通过 Spark-Shell 运行应用程序

  - 准备输入文件

    由于之前的实验过程中很多次传输过所需文件，这里只需要删除一下`spark/output`和目录下的文件。

    ![prepare-file](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/prepare-file.png)

  - 进入 Spark-Shell

    ```shell
    $ ~/spark-2.4.4/bin/spark-shell --master yarn
    ```
    
    **出现问题**,如下:
    
    ![problem_2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/problem_2.png)
    
    查出原因是hadoop的配置文件中有的`Configuration`里设置了<name>和<value>但是没有写<property>框架:
    
    ![problem_2_solution](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/problem_2_solution.png)
    
    解决方法为将之修改为:
    
    ![problem_2_solved](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/problem_2_solved.png)
    
    重启hadoop-2.9.2下的yarn.再次运行,出现**另一个问题**:
    
    ![problem_3](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/problem_3.png)
    
    问题原因如网站[YARN-4714](https://issues.apache.org/jira/browse/YARN-4714)所说,是一个blocker问题,虚拟内存使用过高.
    
    解决方法为在Hadoop 配置文件 `yarn-site.xml` 中添加取消虚拟内存的检查设置,具体如下图所示:
    
    ![problem_3_solution](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/problem_3_solution.png)
    
    再次格式化,重新运行.成功,结果如下:
    
    ![problem_3_solved](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/problem_3_solved.png)
    
  - 在 `scala>` 后输入 Scala 代码.
    
  ```scala
    scala> sc.textFile("hdfs://localhost:9000/user/ecnu/spark_input/RELEASE").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).collect
  ```
  
    运行成功,结果如下:  ![scala_3](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/scala_3.png)
  
- 通过提交 Jar 包运行应用程序

  - Client 提交模式 (默认), 此模式下 Driver 运行在客户端, 可以在客户端看到应用程序运行过程中的信息

    ![jar_2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/jar_2.png)
  
    成功,结果如下:
  
    ![jar_2_result](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/jar_2_result.png)
  
    在另一个`terminal`中运行`jps`,查看此时运行的进程:
  
    ![jar_client_status](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/jar_client_status.png)
  
- Cluster 提交模式, 此模式下 ResourceManager 会随机选取一个 NodeManager 所在节点在其上启动 ApplicationMaster.由于Driver 运行在 ApplicationMaster 中, 故在客户端看不到应用程序运行过程中的信息:
  
  ![jar_cluster_exec](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/jar_cluster_exec.png)
  
  在另一个`terminal`中运行`jps`,查看此时运行的进程:
  
  ![jar_cluster_status](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/jar_cluster_status.png)

#### 1.3 查看 Spark 程序运行信息

- 实时查看应用运行情况

  在应用运行过程中 (如进入 Spark-Shell 之后), 访问 http://localhost:4040

  - Client:

    ![web_client](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/web_client.png)

  - Cluster:

    由于Driver不是运行在本地的,所以没法登陆.

    ![web_cluster](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/web_cluster.png)

- 查看 Spark 应用程序日志

  在提交一个应用程序后，在 `~/hadoop-2.9.2/logs/userlogs` 下会出现应用程序运行日志

  ![logs_](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/logs_.png)

- 查看应用历史记录

  在应用运行结束后, 访问 http://localhost:18080
  
  <img src="C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/web_logs_.png" alt="web_logs" style="zoom:80%;" />

#### 1.4 停止服务

![stop_](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-3/stop_.png)

### 2 分布式部署

#### 2.1 准备工作与启动服务

- 启动命令 (在 Yarn 主节点上执行)，并在主节点的terminal中执行`jps`来验证是否成功执行服务

  ```shell
  ~/hadoop-2.9.2/sbin/start-yarn.sh
  ~/hadoop-2.9.2/sbin/mr-jobhistory-daemon.sh start historyserver
  ~/hadoop-2.9.2/sbin/start-dfs.sh
  ~/spark-2.4.4/sbin/start-history-server.sh
  
  jps
  ```
  
  运行成功，结果如下：
  
  ![jps_dos_yarn](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jps_dos_yarn.png)

#### 2.4 运行 Spark 应用程序

- 通过 Spark-Shell 运行应用程序

  - 准备输入文件

    将本地的 `~/spark-2.4.4/RELEASE` 上传至 HDFS 的 `/user/you/spark_input` 下

  - 进入 Spark-Shell

    ```shell
    $ ~/spark-2.4.4/bin/spark-shell --master yarn
    ```
    
    ![spark_shell_dos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\spark_shell_dos.png)
    
    并在 `scala>` 后输入 Scala 代码.
    此处执行的是统计 `RELEASE` 文件中的单词数量
    
    ```scala
    scala> sc.textFile("hdfs://219.228.135.148:9000/user/ecnu/spark_input/RELEASE").flatMap(_.split(" ")).map((_,1)).reduceByKey(_+_).collect
    ```

    结果如下：
    
    ![spark_shell_scala_example_1](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\spark_shell_scala_example_1.png)

- 通过提交 Jar 包运行应用程序

  - Client 提交模式 (默认), 此模式下 Driver 运行在客户端, 可以在客户端看到应用程序运行过程中的信息

    ```shell
    >>> ~/spark-2.4.4/bin/spark-submit \
    >>>   --deploy-mode client \
    >>>   --master yarn \
    >>>   --class org.apache.spark.examples.SparkPi \
    >>>   ~/spark-2.4.4/examples/jars/spark-examples_2.11-2.4.4.jar
    ```

    ![jar_pi_dos_client_satat](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_pi_dos_client_satat.png)

  ![jar_pi_dos_client_inf](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_pi_dos_client_inf.png)

    ![jar_pi_dos_client_result](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_pi_dos_client_result.png)
  
    同时，在运行过程中另起一个终端执行 `jps` 查看进程。
  
    结果如下：
      
    ![jps_jar_dos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jps_jar_dos.png)

  - Cluster 提交模式, 此模式下 ResourceManager 会随机选取一个 NodeManager 所在节点在其上启动 ApplicationMaster,
    Driver 运行在 ApplicationMaster 中, 故在客户端看不到应用程序运行过程中的信息
  
    ```shell
    ~/spark-2.4.4/bin/spark-submit \
      --deploy-mode cluster \
      --master yarn \
      --class org.apache.spark.examples.SparkPi \
      ~/spark-2.4.4/examples/jars/spark-examples_2.11-2.4.4.jar
    ```

  
  ![jar_pi_dos_cluster_start](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_pi_dos_cluster_start.png)
  
  ![jar_pi_dos_cluster_inf](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_pi_dos_cluster_inf.png)
  
  ![jar_pi_dos_cluster_result](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_pi_dos_cluster_result.png)
  
    同时，在运行过程中另起一个终端执行 `jps` 查看进程。
  
    结果如下：
  
    ![jps_jar_cluster_dos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jps_jar_cluster_dos.png)

#### 2.5 查看 Spark 程序运行信息

- 实时查看应用运行情况

  在应用运行过程中，访问 http://219.228.135.148:4040。得到如下结果：

  ![web_jar_dos_exec](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\web_jar_dos_exec.png)

- 查看 Spark 应用程序日志

  不知为何，我的spark分布式运行变成了standalone的模式，所有的logs都存在了`NodeManager节点的 spark安装目录/work/目录`，结果如下：

  ![logs_pc](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\logs_pc.png)

- 查看应用历史记录

  在应用运行结束后, 访问 http://219.228.135.148:18080
  
  ![web_logs_dos](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\web_logs_dos.png)

#### 2.6 停止服务

- 停止命令 (在 Yarn 主节点上执行)

  ```shell
  ~/spark-2.4.4/sbin/stop-history-server.sh
  ~/hadoop-2.9.2/sbin/stop-yarn.sh
  ~/hadoop-2.9.2/sbin/mr-jobhistory-daemon.sh stop historyserver
  ~/hadoop-2.9.2/sbin/stop-dfs.sh
  ```

## 四. Spark-Streaming-Programming

### 1 编写程序

新建 Maven 项目，设置依赖，添加类并编程。

#### 1.1 IDEA 中运行

- 安装 Netcat (`yum install nc`), 并选择一个端口号 (如8888) 开启 Netcat 服务 (`nc -lk 8888`)

  遇到问题：

  ![problem_1](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_1.png)

  这种情况出现的原因是`yum`所需python版本不兼容，于是修改`/usr/bin/yum`：

  ![problem_1_solution](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_1_solution.png)

  再次运行，发现可以运行，但是需要重新配置`repositories`，非常繁琐，于是直接使用`apt`，也便于管理，运行成功，结果如下：

  ![problem_1_solution_2](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_1_solution_2.png)

  ![problem_1_solution_3](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_1_solution_3.png)

- 配置运行环境，设IP 地址为开启 Netcat 的 IP, 端口号为开启 Netcat 的端口号。运行程序，成功。结果如下：

  ![problem_1_solved](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_1_solved.png)

#### 1.2 提交 Spark 程序

- 将程序打成 jar 包并在伪分布模式下提交程序

  ![problem_2_pre](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_2_pre.png)

  出现问题：

  ![problem_2_](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_2_.png)

  出现这种情况的原因是NameNode发现集群中DataNode丢失达到一定比例，于是会进入安全模式，此时只允许查看数据不允许对数据进行任何操作。解决方法为：

  ![problem_2_solution](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_2_solution.png)

  之后重新运行，成功。结果如图：

  ![problem_2_solved_](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\spark-0\spark-streaming/problem_2_solved_.png)

- 分布式模式下提交程序

  ```shell
  ~/spark-2.4.4/bin/spark-submit \
    --master spark://219.228.135.148:7077 \
    --class StreamingWordCount \
    /home/ecnu/Downloads/StreamingWordCount.jar 219.228.135.148 8888 hdfs://219.228.135.148:9000/user/ecnu/spark_output
  ```
  遇到问题：
  
  ![problem_3](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_3.png)
  
  问题的描述是`ip`地址为`219.228.135.148`（即`Master`）端口为7077的节点无法访问，通过`nmap`判断7077是否被占用：
  
  ![problem_3_condition](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_3_condition.png)
  
  发现并没有，于是重新审理整个过程，发现`master`和`slaves`没有启动，于是重新启动spark：
  
  ![problem_3_solution](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\problem_3_solution.png)
  
  重新运行，成功。结果如下：
  
  ![jar_dos_result](C:\Users\wushu\Documents\temperate\dos\实验\2019-lecture\pj2\ecnu\jar_dos_result.png)

