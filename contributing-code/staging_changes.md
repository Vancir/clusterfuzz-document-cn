---
layout: default
title: Staging changes
parent: Contributing code
nav_order: 3
permalink: /contributing-code/staging-changes/
---

# Staging changes


建议先暂存(stage)在App Engine或Bot上进行的代码改动, 然后再将其部署到生产环境中. 这样有助于在大规模部署这些改动之前, 获得这些改动的造成的影响. 

- TOC
{:toc}
---


## Prerequisites

* 您首先需要参照[生产设置]篇章部署好一个环境. 

## App Engine changes

您可以使用以下命令在暂存服务器实例上测试UI或Cron的改动:

```bash
python butler.py deploy --staging --config-dir=$CONFIG_DIR
```

部署后, 可以前往`https://staging-dot-<your project id>.appspot.com`查看您的改动.

**注意**: 暂存服务器使用与生产服务器相同的数据库. 因此，请注意任何可能影响生产数据库内数据的改动. 


## Bot changes

您可以使用以下方法在特定的Compute Engine Bot上测试代码改动: 

```bash
python butler.py remote \
  --instance-name <your instance name> \
  --project <your project id> \
  --zone <your project zone> \
  stage --config-dir=$CONFIG_DIR
```

**注意**:
* 这些更改在2天之内都不会被任何生产部署所覆盖, 以便您有足够的时间来测试您的更改. 如果要放弃这些更改, 只需重新启动Bot即可. 
* 目前只有通过*docker*目录所提供的docker映像运行起来的Google Compute Engine Bot才支持此功能. 

[生产设置]: {{ site.baseurl }}/production-setup/
