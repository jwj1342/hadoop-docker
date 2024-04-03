## Hadoop Docker实现

基于的原始仓库地址：https://github.com/kiwenlau/hadoop-cluster-docker

### 快速使用
1. image的构建：`docker pull kiwenlau/hadoop:1.0`

2. hadoop网络创建：`docker network create --driver=bridge hadoop`

3. 构建一个新的Docker Image（科学上网注意）:

   ```bash
   docker build -t kiwenlau/hadoop:1.0 .
   ```
4. 运行一个新的Docker容器

    ```bash
    docker run -itd --net=hadoop -p 50070:50070 -p 8088:8088 --name hadoop-master --hostname hadoop-master kiwenlau/hadoop:1.0
    ```

5. 类似于第二条命令，但是这次创建一个名为 `hadoop-slavex`

    ```bash
    docker run -itd  --net=hadoop --name hadoop-slave1  --hostname hadoop-slave1 kiwenlau/hadoop:1.0
    
    docker run -itd  --net=hadoop --name hadoop-slave2  --hostname hadoop-slave2 kiwenlau/hadoop:1.0
    
    docker run -itd  --net=hadoop --name hadoop-slave3  --hostname hadoop-slave3 kiwenlau/hadoop:1.0
    
    docker run -itd  --net=hadoop --name hadoop-slave4  --hostname hadoop-slave4 kiwenlau/hadoop:1.0
    ```

6. 在名为 `hadoop-master` 的容器内启动一个bash会话。

    ```bash
    docker exec -it hadoop-master bash
    ```

7. 直接使用`./start-hadoop.sh`启动服务，或者先`echo`命令找到`$HADOOP_HOME`的安装位置，然后后面接上`/sbin/start-dfs.sh`以及`/sbin/start-yarn.sh`

### 实现细节

在实现master部分的时候： `-itd` 参数是 `-i`、`-t` 和 `-d` 的组合，分别代表交互模式、分配一个伪终端和在后台运行。`--net=hadoop` 指定容器使用的网络是 `hadoop`。`-p 50070:50070 -p 8088:8088` 是端口映射，将容器的50070端口映射到宿主机的50070端口，8088同理。`--name` 指定容器名称为 `hadoop-master`，`--hostname` 设置容器内的主机名为 `hadoop-master`。
