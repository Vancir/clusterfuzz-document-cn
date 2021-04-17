---
layout: default
title: Architecture
permalink: /architecture/
nav_order: 1
parent: ClusterFuzz
---

# Architecture
{: .no_toc}
![overview]({{ site.baseurl }}/images/overview.png)

ClusterFuzz提供了一个自动化的端到端基础设施, 用于查找和分类崩溃, 最大程度地减少崩溃的重现, [二分选择]()和验证修复代码. 

- TOC
{:toc}

---

## Supported platforms

ClusterFuzz使用Python编写. 它可运行在**Linux**, **macOS**, 和 **Windows**平台上.

## Requirements

ClusterFuzz运行在[Google Cloud Platform](https://cloud.google.com/), 并且依赖了多种服务:

- Compute Engine (并非绝对需要. [Fuzzing bots](#fuzzing-bots)可在任何地方运行.)
- App Engine
- Cloud Storage
- Cloud Datastore
- Cloud Pub/Sub
- BigQuery
- Stackdriver日志记录及监控

**注意:** 我们当前仅支持使用托管了Chromium的[Monorail]()作为Bug跟踪器. 将来会增加对自定义Bug跟踪器对支持.

### Local instance

通过使用本地的Google Cloud模拟器, 您可以在没有这些依赖项的情况下在本地运行ClusterFuzz. 但如果使用模拟器的话, 某些依赖BigQuery和Stackdriver的功能会因为缺少模拟器支持而被禁用. 

**注意:** 本地实例仅支持**Linux**和**macOS**平台.

## Operation

ClusterFuzz包含有两大主要组件: 

- App Engine 实例
- [模糊测试机器人]({{ site.baseurl }}/reference/glossary/#bot)池

### App Engine

App Engine实例提供了一个Web界面来获取崩溃信息, 统计信息和其他信息. 它还负责安排定时任务. 

### Fuzzing Bots 

模糊机器人负责从所在平台对应到任务队列中获取任务并运行, 机器人主要运行的任务有: 

- `fuzz`: 运行一个模糊测试的会话. 
- `progression`: 检查一个测试用例是否依然可以重现崩溃或者已经修复. 
- `regression`: 计算引入崩溃的代码修订范围.
- `minimize`: 执行测试用例[最小化]().
- `corpus_pruning`: 根据覆盖率将[语料库]({{ site.baseurl }}/reference/glossary/#corpus)缩减到最小(仅限libFuzzer). 
- `analyze`: 针对某个任务手动上传测试用例并运行, 查看其是否会崩溃.

ClusterFuzz上包括两种机器人: 

- **抢占式**: 抢占式机器可以随时关闭, 并且只能运行`fuzz`任务. 云供应商通常以更低廉的价格提供抢占式实例, 因此我们建议使用这种类型的机器人进行扩展. 

- **非抢占式**: 非抢占式机器不应被关闭, 它可以运行所有功能的任务, 包括必须不间断运行的关键性任务比如`progression`. 


[bisecting]: https://en.wikipedia.org/wiki/Bisection_(software_engineering)
[minimization]: {{ site.baseurl }}/reference/glossary/#minimization
