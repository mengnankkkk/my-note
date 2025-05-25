---
title: Mysql进阶技术栈
categories:
  - sql
abbrlink: 2708
date: 2025.5.21
tags: 
   - mysql 
   - 面试
---

# 主从同步

## 1.MySQL 主从架构有什么优缺点？

MySQL Replication 是 MySQL 官方提供的主从同步方案，用于将 MySQL 主库的数据同步到从库中，从库可以供应用程序读取数据。

Replication 是目前 MySQL 使用最多的灾备方案，主要有 3 个作用：

1. **读写分离**，写主库读从库。这样大大降低主库的负载，即使主库出现类似锁表之类的情况，也不影响应用读取数据。
2. **实现灾备**，当主库发生故障时，可以方便地把从库切换成主库，实现**高可用**（HA）。
3. **水平扩展**，当应用访问量导致数据库 I/O 高时，可以通过水平扩展的方式将降低单机负载，降低磁盘 I/O。

**同步原理：**

MySQL 通过 binlog 实现同步过程中，会用到 3 个线程：

- **IO thread**: 从库执行 START SLAVE 语句时，会创建一个 IO thread，负责连接主节点，请求更新的 **binlog****，接收到 binlog 后写入 relaylog；**
- dump thread：主库接收到从库的 binlog 请求后，创建一个 **dump thread**，把 binlog 同步给从库；
- sql thread：**读取 relaylog，解析 relaylog 的命令并执行，将数据落库。**

![](https://mmbiz.qpic.cn/mmbiz_png/a1gicTYmvicd8A2qPg567sK8NyS4KJ7rn97XibbwjO9bs8gTv2ODQNRyT8W98jvmGzgYVyBibLELp2nkdp9eT5BPjg/640?wx_fmt=png&from=appmsg&tp=wxpic&wxfrom=5&wx_lazy=1)

在从库上执行 **change master 命令，设置要连接主库的用户名、密码、ip、端口以及请求同步的 binlog 中的位置，这个位置包含文件名和binlog offset；**

从库执行 **start slave** 命令，这时会启动上面的 **IO thread 和 sql thread**，其中 IO thread 负责跟主库建立连接；

主库收到从库的连接请求后，校验用户名密码；

主库校验通过后创建 **dump thread，按照从库请求 binlog 的 offset 将 binlog 发给从库；**

从库收到主库发送的 binlog 后，将日志写入 **relaylog；**

sql thread 读取 relaylog，解析出命令后执行。

**缺点：**

**最大的缺点就是主从延迟**。

原因：：：：：

从库所在机器性能差，命令执行慢；

从库查询压力大，消耗了大量 CPU 资源，影响了 sql thread 执行；

主库有**大事务**（比如大表DDL），这个事务里面执行的 sql 比较多，一方面主库需要等待事务执行完成才能写入 binlog，另一方面同步到从库和在从库执行都需要花费很多时间，导致主从延迟；

数据库版本低，**在 MySQL 5.6 之前，只支持单线程复制，效率比较低；**

表上无主键，主库利用索引更改数据，从库只能用全表扫描。

要解决主备延迟的问题，可以考虑下面方法：

1. 优化业务逻辑，避免使用大事务，或者大事务场景尽量放在业务低峰期执行；
2. 提高从库所在机器的性能；
3. 保障网络性能，避免网络延迟；
4. 引入 **semi-sync 半同步复制**，配合异步复制。

**主从同步的第二个缺点就是数据丢失**。

> MySQL 有 3 种主从复制方式：
>
> 1. **异步复制**：主**库执行完客户端提交的事务后立即将结果返回给客户端，不关心从库是否同步完成**。这种方式很容易发生数据丢失，比如主库的日志还未同步给从库就宕机了，这时需要在从库中选择一个作为新主库，之前未同步完成的数据就丢失了；
> 2. 全同步复制：**主库执行完客户端提交的事务并且等待从库也执行完成数据同步后再把结果返回给客户端**。这种方式能够保证不丢失数据，但是数据库的性能会受到影响；
> 3. 半同步复制：是介于全同步和异步复制的一种方式，**主库至少等待一个从库接收 binlog 并成功写入到 relaylog 后给客户端返回结果**。主库不需要等待所有从库返回 ACK。

MySQL 中默认采用**异步复制**，这样很容易导致数据丢失。一个好的方式就是采用 **semi-sync 半同步复制插件**。不过 semi-sync 存在一个问题，主库写数据到 binlog 后执行 commit，才会给从库同步数据。如果从库还没有返回 ACK，主库发生了宕机，从库还没有写完 relaylog 就被选择为主库，也会发生数据丢失。

MySQL 5.7 引入了增强版**半同步复制**。主库写入数据到 binlog 后，就给从库进行同步，直到至少一个从库返回给主库 ACK，主库才会进行 commit 操作。

## 2.**MySQL双主架构有什么优缺点？**

双主架构是 MySQL 常见的一种架构模式，它的特点是**有两个主节点对外提供服务，并且这两个主节点互为主备**。今天来学习一下双主架构。

### 1.双主复制

这种架构的特点配置两个主库，**每个主库都提供读写服务，并且这两个主库互为主备。**如下图：

![](https://mmbiz.qpic.cn/mmbiz_png/a1gicTYmvicd9JI16DxScxchEPaXKy2zRrpict4icZnFhokHazh1k4LyCP2J5YBkvTjA9WmzAJoLiabXOZaqfcG25jg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&tp=wxpic)

在 M1 写入的数据要同步到 M2，在 M2 写入的数据要同步到 M1。这种两个主库同时支持写入，这种架构模式一个明显的优势写入效率高。比如一个应用在不同的城市部署了两个主节点，请求可以就近选择写入数据库。

但这种架构在数据同步时很容易出问题。 

案例一：**M1 和 M2 同时收到一张表的插入请求，这张表是自增主键，两张表插入后主键相同。这时发生数据同步，这条插入语句在 binlog 里面记录的是 row 格式，同步时发生主键冲突。**

> MySQL binlog 有三种格式：
>
> - STATEMENT：记录的是 SQL 语句本身；
> - ROW：记录的是数据的变化；
> - MIXED：STATEMENT 和 ROW 格式的结合，MySQL 会根据 SQL 语句特性选择使用 STATEMENT 还是 ROW 格式。

解决方法：：：

MySQL 5.0 后可以通过设置 **auto_increment_increment** 和 **auto_increment_offset** 这两个选项来解决这个问题。

案例二：在 M1 上执行了一条语句，生成 binlog 后发给 M2 进行同步，M2 执行完成后又生成 binlog 同步给 A，**导致一条语句循环复制**。



这个问题的解决方法是要求 **M1 和 M2 的 server id**不相同，M1 产生的 binlog 记录 server id 是 M1，M2 执行同步时生成的 binlog 也记录 server id 为 M1。这样同步给 M1 是，M1 判断到 server id 跟自己相同，就丢弃这个日志，不做同步。

案例三：**同步过程中会有数据不一致的问题**。比如用户 xiaoming 的账户余额是 100。M1 执行了 update 操作把账户余额更新成 150，M2 执行了 update 操作更新成 130。

解决这种数据不一致问题的一个思路是**严格划分数据和设置权限**，比如案例中小明的所有数据只能在 M1 上操作。

案例四：**因为节点发生故障，M1 不能复制了**，但是应用可以写数据库，M2 能正常写和复制，这个问题就很难解决了。

解决这个问题，**需要给 M1 和 M2 配置从节点，主节点故障后切换到从节点进行工作。**

### 2.主备复制

这个架构模式的特点是双主节点中，**同一时刻只有一个主节点提供写服务，另一个主节点只能提供读服务。**如下图：

![](https://mmbiz.qpic.cn/mmbiz_png/a1gicTYmvicd9JI16DxScxchEPaXKy2zRr1tqZkws2VGynmsyOtvbm3ia5VxnjCf6klRRyTChQo1dsXn5FsK1SJwA/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&tp=wxpic)

这个架构相当于比单主节点架构多了一个热备，有如下优势：

1. 因为 M1 和 M2 配置对称，切换主备比较容易；
2. 有助于故障转移和恢复；
3. 可以在不影响应用的情况下进行数据库升级和维护；
4. 不用考虑循环复制问题和主备不一致问题。

当然，主备架构也有缺点，**那就是写性能不能得到提升。**

### 3.主主架构拥有备库

主主架构中每个主库也可以拥有备库，如下图：

![](https://mmbiz.qpic.cn/mmbiz_png/a1gicTYmvicd9JI16DxScxchEPaXKy2zRrpict4icZnFhokHazh1k4LyCP2J5YBkvTjA9WmzAJoLiabXOZaqfcG25jg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&tp=wxpic)

这种配置为每个主库增加了一个备份，**可以防止单点故障，同时备库也可以处理读请求，提高数据库整体读效率。**

这个架构的缺点是增加了机器成本。

### 4.环形复制

环形复制架构是 MySQL 集群中拥有多个主库，主库之间形成一个环形，**前面一个节点是当前节点的主库，当前节点是前面节点的备库，也是后面一个节点的主库**。如下图：

![](https://mmbiz.qpic.cn/mmbiz_png/a1gicTYmvicd9JI16DxScxchEPaXKy2zRrZm1OfPH5HMCAyzHxCdicCmrh98dL9p1uWnLlIbAFhXH1ibAic8icxv8jyg/640?wx_fmt=png&from=appmsg&wxfrom=5&wx_lazy=1&tp=wxpic)

环形复制这种架构其实并不推荐，**因为它很难做到故障转移**，高可用特性依赖于每个节点不出故障。但是如果一个节点出了故障，去掉这个节点，**这个节点产生的 binlog 将一直循环复制下去，因为只有通过这个节点的 server id 才能做出判断停止复制。**

