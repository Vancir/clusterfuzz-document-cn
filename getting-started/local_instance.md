---
layout: default
title: Running a local instance
parent: Getting started
nav_order: 2
permalink: /getting-started/local-instance/
---

# Local instance of ClusterFuzz
{: .no_toc}

您可以运行ClusterFuzz的本地实例来测试核心功能. 请注意, 由于缺少Google Cloud模拟器, 某些功能(例如[崩溃]()和[模糊器统计信息]())在本地实例中被禁用. 

- TOC
{:toc}

[crash]: {{ site.baseurl }}/using-clusterfuzz/ui-overview/#crash-statistics
[fuzzer statistics]: {{ site.baseurl }}/using-clusterfuzz/ui-overview/#fuzzer-statistics

---

## Running a local server

您可以通过运行以下命令来启动一个本地服务器:

```bash
# 如果您是第一次运行该服务器或则想要重置所有的数据.
python butler.py run_server --bootstrap

# 在所有其他的情形下, 请勿使用"--bootstrap"标志.
python butler.py run_server
```

一开始可能需要花费几秒钟启动. 一旦您看到类似`[INFO] Listening at: http://0.0.0.0:9000`的输出信息时, 您就可以通过导航到[http://localhost:9000](http://localhost:9000)查看网页界面.

**注意:** 本地实例可以使用[9000以外](https://github.com/google/clusterfuzz/blob/master/src/local/butler/constants.py)的其他端口(例如9008)来进行文件上传等操作. 如果所需的端口不可用, 或者您只能从浏览器访问某些所需的端口(例如, 从另一台主机访问时具有端口转发或防火墙规则), 则您的本地实例可能会被中断.

## Running a local bot instance

您可以通过运行以下命令在本地实例中运行ClusterFuzz机器人:

```bash
python butler.py run_bot --name my-bot /path/to/my-bot  # 将my-bot命名成其他名称
```


该命令会在`/path/to/my-bot/clusterfuzz`下创建ClusterFuzz的代码副本, 并使用它来运行bot. 大多数机器人组件(例如日志，模糊器和语料库)都在bot子目录中创建.

如果您打算对原生GUI应用程序进行模糊测试, 建议您在虚拟帧缓冲区(如[Xvfb](https://en.wikipedia.org/wiki/Xvfb))中运行此命令. 否则, 您将在模糊测试过程中看到GUI对话框.

### Viewing logs

您可以通过运行以下命令查看本地实例上的日志:

```bash
cd /path/to/my-bot/clusterfuzz/bot/logs
tail -f bot.log
```

**注意:** 在您有设置任何[模糊测试任务]({{ site.baseurl }}/setting-up-fuzzing/)之前, 您会在 日志中看到无害的错误提示: `Failed to get any fuzzing tasks`. 

## Local Google Cloud Storage

我们使用了本地的文件系统对[Google Cloud Storage]进行了模拟. 默认情况下, 它存储在ClusterFuzz项目路径`local/storage/local_gcs`处.

您可以通过执行以下操作覆盖默认位置:

1. 运行`run_server`命令时使用`--storage-path`标志.
2. 运行`run_bot`命令时使用`--server-storage-path`标志来指定相同路径. 

在您指定的位置, 对象存储在`<bucket>/objects/<object path>`处, 元数据存储在`<bucket>/metadata/<object path>`处. 

[Google Cloud Storage]: https://cloud.google.com/storage/
