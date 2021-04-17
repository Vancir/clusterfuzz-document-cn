---
layout: default
title: Prerequisites
parent: Getting started
nav_order: 1
permalink: /getting-started/prerequisites/
---

# Prerequisites
{: .no_toc}

本页面说明了如何设置环境使用ClusterFuzz.

- TOC
{:toc}

---
## Requirements

ClusterFuzz的许多功能依赖于[Google Cloud Platform](https://cloud.google.com) 服务, 但它同样可以无须这些依赖在本地运行起来作为测试用途. 更多细节请查阅[Architecture page]({{ site.baseurl }}/architecture/#requirements)了解.

**注意:** 本地开发环境仅支持**Linux**平台.

## Getting the code

运行以下命令克隆ClusterFuzz到你的计算机上:


```bash
git clone https://github.com/google/clusterfuzz
cd clusterfuzz
git pull
```

出于稳定性考虑, 我们建议您使用代码的[最新发行版本](https://github.com/google/clusterfuzz/releases/latest)(而非master分支). 您可以使用以下命令切换到特定版本:

```bash
git checkout tags/vX.Y.Z
```

其中 X.Y.Z 是具体到发布版本(比如, 1.0.1).

## Installing prerequisites

### Google Cloud SDK

遵循[在线说明](https://cloud.google.com/sdk/)安装Google Cloud SDK.

### Log in to your Google Cloud account

**注意:** 如果您[在本地运行ClusterFuzz](), 那么这部分内容不是"必需"的.

如果您打算[在生产环境中设置ClusterFuzz](), 则应使用`gcloud`工具对您的帐户进行身份验证:

```bash
gcloud auth application-default login
gcloud auth login
```

[set up ClusterFuzz in production]: {{ "/production-setup/" | relative_url }}
[running ClusterFuzz locally]: {{ "/getting-started/local-instance/" | relative_url }}

### Python programming language

[下载 Python 3.7](https://www.python.org/downloads/release/python-377/)并安装. 如果已经安装好了Python, 则可以通过运行`python --version`来验证版本.

我们建议使用官方仓库中的python源码进行构建, 因为它会安装所需的python头文件和pip. 否则, 请确保明确安装了它们. 

### Go programming language

[Install the Go programming language](https://golang.org/doc/install).

### Other dependencies

我们提供了一个脚本, 用于在Linux和macOS上安装所有其他的开发依赖项. 


我们支持的系统包括有:

- **Ubuntu** (14.04, 16.04, 17.10, 18.04, 18.10)
- **Debian** 8 (jessie) 或更高版本
- 安装有[homebrew]的**macOS**最新版本(实验性)

要安装依赖项, 请运行以下脚本:

```bash
local/install_deps.bash
```

[homebrew]: https://brew.sh/

## Loading pipenv

在运行完`local/install_deps.bash`脚本之后, 通过运行以下命令激活pipenv:

```bash
pipenv shell
```

该命令会载入所有到Python依赖到当前到环境中去.

您可以运行以下命令验证环境是否配置成功.

```bash
python butler.py --help
```
