---
title: 分布式服务唯一ID生成
date: 2017-08-30 15:53:23
categories:
- 分布式
tags:
- 分布式
---


#### 唯一ID生成要求

###### 基本要求
1. 全局唯一性：不能出现重复的ID号，既然是唯一标识，这是最基本的要求。
2. 信息安全：如果ID是连续的，恶意用户的扒取工作就非常容易做了，直接按照顺序下载指定URL即可；如果是订单号就更危险了，竞对可以直接知道我们一天的单量。所以在一些应用场景下，会需要ID无规则、不规则。
3. 高可用。业务对ID生成系统可用性要求极高，如果ID生成系统瘫痪，会导致整个系统无法使用。
4. 高并发: 整个系统都依赖ID生成系统，ID生成系统的并发决定了系统并发的上限。

###### 其他要求
1. 递增: 某些数据库主键递增可以显著提供性能。
2. 时间相关: 某些场景下根据唯一ID得到时间有利于查询统计。


#### 唯一ID生成方案

#### 自增-数据库
#### UUID
#### snowflake算法

#### 优秀博客
[Leaf——美团点评分布式ID生成系统](https://tech.meituan.com/MT_Leaf.html)