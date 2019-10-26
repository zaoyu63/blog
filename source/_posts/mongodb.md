---
title: linux安装mongodb
date: 2019-10-26 20:51:51
tags: mongodb NoSQL
---
# 简介

是一个基于分布式文件存储的数据库。由 C++ 语言编写。旨在为 WEB 应用提供可扩展的高性能数据存储解决方案   
是一个介于关系数据库和非关系数据库之间的产品，是非关系数据库当中功能最丰富，最像关系数据库的。

# SQL与NoSQL

### SQL（关系型数据库）

* 关系型数据库遵循ACID规则 事物 原子性、一致性、独立性、持久性
* 存储的是行记录，无法存储数据结构
* 可以用SQL语句方便的在一个表以及多个表之间做非常复杂的数据查询
* 使得对于安全性能很高的数据访问要求得以实现
* 不易水平扩展

### NoSQL（非关系型数据库）
* 缺少事务
* 大数据
* 易扩展

# 安装

下载地址：https://www.mongodb.com/download-center#community

1. 使用wget下载

```$xslt
wget https://fastdl.mongodb.org/linux/mongodb-linux-x86_64-3.0.6.tgz
```

2. 解压
```$xslt
tar -zxvf mongodb-linux-x86_64-3.0.6.tgz 
```

3. 移动

```$xslt
mv  mongodb-linux-x86_64-3.0.6/ /usr/local/mongodb   
```
4. 配置环境变量
```$xslt
vi /etc/profile
```
在 export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL 一行的上面添加如下内容:
```$xslt
#Set Mongodb
export PATH=/usr/local/mongodb/bin:$PATH
```
保存后通过下面的命令使环境变量生效
```$xslt
$ source /etc/profile
```
5. 创建数据库目录  
放在 /user/local/data 里  
目录结构如下
```$xslt
 data  
    conf
        mongodb.conf
    db
    log
        mongodb.log
```
代码
```$xslt
cd /usr/local
mkdir data data/conf data/log data/db
touch data/conf/mongodb.conf
touch data/log/mongodb.log
```
6. mongodb.conf配置
添加一下内容
```$xslt
port=27017 #端口
dbpath= /usr/mongodb/db #数据库存文件存放目录
logpath= /usr/mongodb/log/mongodb.log #日志文件存放路径
logappend=true #使用追加的方式写日志
fork=true #以守护进程的方式运行，创建服务器进程
maxConns=100 #最大同时连接数
noauth=true #不启用验证
journal=true #每次写入会记录一条操作日志（通过journal可以重新构造出写入的数据）。
#即使宕机，启动时wiredtiger会先将数据恢复到最近一次的checkpoint点，然后重放后续的journal日志来恢复。
storageEngine=wiredTiger  #存储引擎有mmapv1、wiretiger、mongorocks
bind_ip = 0.0.0.0  #这样就可外部访问了，例如从win10中去连虚拟机中的MongoDB
```
7. 启动 mongodb
```$xslt
$ mongod --config /usr/data/conf/mongodb.conf # --config 可用 -f 代替
```
