---
title: docker安装kafka集群
---

## 新建一个docker-compose.yml文件内容如下  
```bash
#vim docker-compose.yml
version: '2'
services:
  broker1:
    image: wurstmeister/kafka
    container_name: broker1
    ports:
      - "19011:22"
      - "9097:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 127.0.0.1
      KAFKA_ZOOKEEPER_CONNECT: 127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183
    restart: always
  broker2:
    image: wurstmeister/kafka
    container_name: broker2
    depends_on:
      - broker1
    ports:
      - "19012:22"
      - "9098:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 127.0.0.1
      KAFKA_ZOOKEEPER_CONNECT: 127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183
    restart: always
  broker3:
    image: wurstmeister/kafka
    container_name: broker3
    depends_on:
      - broker2
    ports:
      - "19013:22"
      - "9099:9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 127.0.0.1
      KAFKA_ZOOKEEPER_CONNECT: 127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183
    restart: always
  kafka-manager:
    image: sheepkiller/kafka-manager                ## 镜像：开源的web管理kafka集群的界面
    environment:
        ZK_HOSTS: 127.0.0.1:2181,127.0.0.1:2182,127.0.0.1:2183                   ## 修改:宿主机IP
    ports:
      - "9000:9000"
```

## 运行  
```bash
docker-compose up -d
```
