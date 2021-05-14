---
layout: default
title: Source code
parent: Contributing code
nav_order: 1
permalink: /contributing-code/source-code/
---

# Source code

- TOC
{:toc}
---


## Directory structure

* **bot** - 放置bot特定文件(例如fuzzers, corpora)的目录. 
* **configs** -示例项目配置, 也用于测试. 
* **coverage** - 用于存储测试的覆盖率数据的目录. 
* **deployment** - 存储部署相关的文件, 例如源代码归档. 
* **docker** - 示例Docker映像示例, 测试于Google Cloud VMs. 
* **docs** - 本文档. 
* **local** - 用于本地测试的文件(例如, 用于安装ClusterFuzz依赖项的install_deps.bash).
* **resources** - Bots所需的辅助用二进制文件(例如llvm-symbolizer).
* **src** - 源代码根目录.
  * **appengine** - App Engine相关代码/文件(Python 2)
    * **handlers** - 各种网页的后端代码. 
    * **libs** - 通用的辅助用代码库(例如访问控制).
    * **private** - 各种网页的原始模板代码(Polymer 2)
    * **templates** - 用于自动生成各种网页的最小模板代码(Polymer 2).
  * **go** - Go语言代码.
  * **python** - Python 2代码.
    * **bot** - Bot相关代码. 
    * **tests** - 核心代码和App Engine代码的单元测试集. 
    * 其余的是*代码包package*级别的目录. 
  * **third_party** - 依赖的第三方软件包.
  * **requirements.txt** - 平台无关的依赖包清单(Python 2). 
  * **platform_requirements.txt** - (Python 2). 平台相关的依赖包清单(Python 2).
* **bower.json** - 用于Polymer 2的组件依赖. 
* **butler.py** - 用于各种命令行任务(例如测试, 部署)的帮助程序脚本.
