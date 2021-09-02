---
layout: default
title: Access control
parent: Advanced features
grand_parent: Using ClusterFuzz
permalink: /using-clusterfuzz/advanced/access-control/
nav_order: 1
---

# Access control

本页面解释了如何限制对ClusterFuzz网络界面各个部分的访问。这可以通过将用户定义分组来控制。

- TOC
{:toc}
---

## Regular users

普通用户通常是项目中的开发者，他们负责将分类和修复bugs。并且可以访问ClusterFuzz的大部分页面，但权限受到限制。包括：

* 测试用例界面（不包括安全漏洞）
* 测试用例报告界面（不包括安全漏洞）
* crash统计界面（不包括安全漏洞）
* crash范围界面（不包括安全漏洞）
* fuzzer统计界面
* 上传测试用例界面
*  fuzzers界面（仅供参考）
* 语料库界面（仅供参考）
* bots界面

普通用户没有权限访问以下界面：
* job界面
* 配置界面

## Privileged users

特权用户可以不受限制地访问 ClusterFuzz 的大部分界面，并且可以访问所有普通用户可以访问的页面。
特权用户也可以：

* 访问安全漏洞（安全性：在报告中为"YES"）
* 在fuzzers界面上传新的fuzzers
* 在语料库界面上传新的语料
* 在jobs界面创建新的jobs

Privileged users are defined by the [Administrators](#administrators) on the Configuration page.特权用户在配置界面为 [Administrators](#administrators) 权限

## Administrators

 [administrator](https://cloud.google.com/appengine/docs/standard/python/users/adminusers)可以访问ClusterFuzz所有的web界面
特权用户的访问权限还包括：

* 访问配置界面
  * 设置普通用户使用权限：
    * “特权用户”（比如`user@example.com`）
    * “普通用户”
