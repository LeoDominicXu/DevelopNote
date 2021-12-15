# Zookeeper安装（伪分布式）

## 1、下载安装包并解压
```bash
#下载
wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.4.12/zookeeper-3.4.12.tar.gz

#解压
tar -zxvf zookeeper-3.4.12.tar.gz
```
# 2、改配置
```bash
cd $zookeeper_home/conf
cp zoo_sample.cfg zk1.cfg
cp zoo_sample.cfg zk2.cfg
cp zoo_sample.cfg zk3.cfg

vim zk1.cfg #仅修改下面部分，其他默认
dataDir=/data/zookeeper/zk1
clientPort=2181
server.1=127.0.0.1:12888:13888
server.2=127.0.0.1:22888:23888
server.3=127.0.0.1:32888:33888

vim zk2.cfg #仅修改下面部分，其他默认
dataDir=/data/zookeeper/zk2
clientPort=2182
server.1=127.0.0.1:12888:13888
server.2=127.0.0.1:22888:23888
server.3=127.0.0.1:32888:33888

vim zk3.cfg #仅修改下面部分，其他默认
dataDir=/data/zookeeper/zk3
clientPort=2183
server.1=127.0.0.1:12888:13888
server.2=127.0.0.1:22888:23888
server.3=127.0.0.1:32888:33888
```
# 创建必要目录
```bash
mkdir -p /data/zookeeper/zk{1..3}
cd /data/zookeeper/zk1
echo 1 > myid
cd /data/zookeeper/zk2
echo 2 > myid
cd /data/zookeeper/zk3
echo 3 > myid
```

# 创建运行脚本
```bash
vim start.sh
./bin/zkServer.sh start conf/zk1.cfg
./bin/zkServer.sh start conf/zk2.cfg
./bin/zkServer.sh start conf/zk3.cfg
chmod +x start.sh

sh start.sh

#或者jps 确认有没有3个QuorumPeerMain进程
ps aux | grep zookeeper
```

# 登录客户端
```bash
cd $zookeeper_home/bin
./zkCli.sh #便可以连接zookeeper了
```
