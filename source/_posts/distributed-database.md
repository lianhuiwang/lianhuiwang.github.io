---
title: 实现分布式数据库
date: 2016-07-14 16:22:15
tags: 
 - database
 - distributed-computation
categories: 
 - distributed-computation
---
本文主要记录实现一个分布式数据库需要哪些知识和相关的实现点，供有针对性的研究和代码实现。

在数据库领域分为OLAP和OLTP，在单机方面目前主流的OLAP系统有MySQL, PostgreSQL, Informatica, MonetDB/X100, Vectorwise等等。 而在Hadoop上面流行的分布式计算系统有Hive, Impala, Spark, Flink, Drill, Presto, Tez, Tajo, Hawq等等。后续如果有一些流行的系统，在补充这个名单。

分布式数据库是一款能够分布式计算和存储的数据库，首先是一款数据库，那么它就包括数据库的一些步骤，大致有以下：SQL Parser, AST Opt, Logical Optimizer, CBO Optimizer, Physical Plan(DAG) Optimizer, DAG Scheduler等。其中Logical Optimizer和CBO Optimizer是非常重要的。

从分布式执行的角度来看，DAG执行和stage的Pipeline执行方式也是很重要的二个点，主要影响多个结点之间的数据传输(Shuffle)和整个计划(DAG)的执行快慢。Shuffle有Push和Pull二种方式，这也导致DAG执行时是否需要提前将Reduce端的task提前启动。

对于单机的执行来说，就是对Select,Filter,Project,Join,Groupby这几种常见的操作进行优化.表达式Byte code generation和Vector Engine(SIMD), JIT Compiler都是对单机执行的优化，充分利用CPU的优势。还有就是内存管理，对单个task多个操作的内存进行控制，以及在JVM平台上对各个数据类型的内存控制以及减少GC的影响。

对于在分布式计划方面，1. 尽量减少Exchange操作(即shuffle),比如有相同的key上做HashJoin和Groupby操作，可以把二个操作放在一起。2.相同的操作的结果共用，比如多次使用了同一个subquery. 3.CBO优化，比如对于多个Join，哪一个先执行哪一个后执行。

存储方面，主要就是列式存储，目前流行的有Parquet,ORC等，主要任务就是尽可能减少原始数据的扫描量。更深层次的优化就是提前建立好索引，这一块有Elastic Search,Druid,华为的Carbondata等。

在测试方面，大家都一致使用tcpds 99个SQL来进行实际运行测试。
