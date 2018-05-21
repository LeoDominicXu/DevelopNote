# 安装es集群
官网文档地址：https://www.elastic.co/guide/index.html
## 一、创建es用户
ps: 由于es不允许root直接启动，所以需要为es创建一个单独的用户
```bash
groupadd es  #添加用户组
useradd es -g es  #添加用户es
passwd es #为用户es设置密码
```

## 二、安装Java运行环境
es要求Java8（安装方式不做赘述）

## 三、修改系统限制文件
```bash
vim /etc/security/limits.conf
es - nofile 65536#open files
es soft nproc 65536#max user processes
es hard nproc 65536#max user processes
es soft memlock unlimited#virtual memory
es hard memlock unlimited#virtual memory
```

## 四、修改系统配置
```bash
vim /etc/sysctl.conf添加配置信息
vm.max_map_count=262144

/sbin/sysctl -p#生效配置
```
## 五、安装es5
* 下载es，下载地址：https://www.elastic.co/cn/downloads/elasticsearch  
* 解压安装包，tar -zxvf elasticsearch-5.0.0.tar.gz  
* 授权，chown -R es:es elasticsearch-5.0.0

## 六、配置
* es主配置文件[vim config/elasticsearch.yml]  
cluster.name: elasticsearch#集群名，同一个集群中的节点集群名称相同  
node.name: node1#节点名称  
path.data: /data1/es5/#索引数据存放目录，可以指定多个，提升IO性能  
path.logs: /var/log/es5/#日志存放目录  
bootstrap.memory_lock: true#是否开启内存锁定，默认为false。建议开启  
network.host: 10.210.136.34#节点绑定的IP地址  
http.port: 9200#http端口，一台机器启动多个实例时，修改端口  
node.master: true#是否为master节点，默认为true  
node.data: true#是否为数据节点，默认为true  
node.ingest: true#是否为数据转换节点，默认为true  
discovery.zen.ping.unicast.hosts: ["10.0.0.1","10.0.0.2:9300"]#配置集群内其它节点的IP  
discovery.zen.minimum_master_nodes: 3  
#该配置用于集群网络出现异常时，防止脑裂导致的索引数据丢失。配置为：(集群中master节点总数)/ 2 + 1
script.inline: true  
script.stored: true

* 配置JVM参数
【vim config/jvm.options】  
-Xms12g#初始化堆内存大小 具体看机器  
-Xmx12g#堆内存 具体看机器  

## 七、安装ik分词器
ik分词器下载地址：https://github.com/medcl/elasticsearch-analysis-ik/
```bash
mv elasticsearch-analysis-ik-5.0.0.zip %ES_HOME%/plugins/
unzip elasticsearch-analysis-ik-5.0.0.zip -d analysis-ik
chown -R es:es analysis-ik

cd analysis-ik
cp config %ES_HOME%/config
cd %ES_HOME%/config/
mv config ik
chown -R es:es ik
```
## 八、启动es
ps: 以上都是在root用户下操作的，因为es不允许直接用root用户运行，所以现在要切换用户到es用户：
```bash
su es
cd %ES_HOME%
nohup bin/elasticsearch 2>&1 & #后台运行
```

## 九、验证
通过url：http://127.0.0.1:9200/_cat 里面的众多接口接口进行验证。

健康值验证路径：http://127.0.0.1:9200/_cat/health?v, 如果无法连接需要确认配置中network.host配的是啥。
```bash
{
   "cluster_name": "elasticsearch",
   "status": "green",
   "timed_out": false,
   "number_of_nodes": 1,
   "number_of_data_nodes": 1,
   "active_primary_shards": 10,
   "active_shards": 10,
   "relocating_shards": 0,
   "initializing_shards": 0,
   "unassigned_shards": 0
}
```
status健康值的意思：
* green
所有的主分片和副本分片都已分配。你的集群是 100% 可用的。
* yellow
所有的主分片已经分片了，但至少还有一个副本是缺失的。不会有数据丢失，所以搜索结果依然是完整的。不过，你的高可用性在某种程度上被弱化。如果 更多的 分片消失，你就会丢数据了。把 yellow 想象成一个需要及时调查的警告。
* red
至少一个主分片（以及它的全部副本）都在缺失中。这意味着你在缺少数据：搜索只能返回部分数据，而分配到这个分片上的写入请求会返回一个异常。

green/yellow/red 状态是一个概览你的集群并了解眼下正在发生什么的好办法。剩下来的指标给你列出来集群的状态概要：

* number_of_nodes 和 number_of_data_nodes 这个命名完全是自描述的。
* active_primary_shards 指出你集群中的主分片数量。这是涵盖了所有索引的汇总值。
* active_shards 是涵盖了所有索引的_所有_分片的汇总值，即包括副本分片。
* relocating_shards 显示当前正在从一个节点迁往其他节点的分片的数量。通常来说应该是 0，不过在 Elasticsearch 发现集群不太均衡时，该值会上涨。比如说：添加了一个新节点，或者下线了一个节点。
* initializing_shards 是刚刚创建的分片的个数。比如，当你刚创建第一个索引，分片都会短暂的处于 initializing 状态。这通常会是一个临时事件，分片不应该长期停留在 initializing 状态。你还可能在节点刚重启的时候看到 initializing 分片：当分片从磁盘上加载后，它们会从 initializing 状态开始。
* unassigned_shards 是已经在集群状态中存在的分片，但是实际在集群里又找不着。通常未分配分片的来源是未分配的副本。比如，一个有 5 分片和 1 副本的索引，在单节点集群上，就会有 5 个未分配副本分片。如果你的集群是 red 状态，也会长期保有未分配分片（因为缺少主分片）。
