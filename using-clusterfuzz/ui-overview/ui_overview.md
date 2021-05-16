---
layout: default
title: UI overview
has_children: false
parent: Using ClusterFuzz
nav_order: 1
permalink: /using-clusterfuzz/ui-overview/
---

# UI overview
This page gives a brief overview of ClusterFuzz web interface pages. We do not
document every page in detail, as the interface is supposed to be intuitive and
most page elements have tooltips.

本页简要概述了ClusterFuzz Web界面页面。 我们不会详细记录每个页面，因为该界面应该是直观的，并且大多数页面元素都有工具提示。

Once you successfully deployed the server (either locally or in production),
you should be able to access the following pages.

成功部署服务器（本地或生产环境）后，您应该能够访问以下页面。

- TOC
{:toc}
---

## Testcases
This is the default main page. The Testcases page provides information about the
issues that were detected by fuzzers running on ClusterFuzz. The page includes
various filters as well as a search functionality.

这是默认的主页。 “测试用例”页面提供有关在ClusterFuzz上运行的模糊器检测到的问题的信息。 该页面包括各种过滤器以及搜索功能。

## Fuzzer Statistics
The Fuzzer Statistics page provides a variety of metrics about performance of
the fuzzers. For in-process fuzz targets, the page also provides performance
reports and improvement recommendations, as well as links to the metadata
associated with a particular fuzz target.

“ Fuzzer统计信息”页面提供了有关Fuzzer性能的各种指标。 对于过程中的模糊目标，该页面还提供性能报告和改进建议，以及指向与特定模糊目标相关联的元数据的链接。

## Crash Statistics
This page provides various statistics about the crashes that were detected by
ClusterFuzz. These statistics include frequency of crashes, platforms affected,
trends over time.

该页面提供有关ClusterFuzz检测到的崩溃的各种统计信息。 这些统计信息包括崩溃频率，受影响的平台，随时间变化的趋势。

## Upload Testcase
This page provides a convenient way to upload a single testcase to be tested
with a specific target. The most common use case for this is to test a bug that
was reported by an external researcher or found by some other system.


该页面提供了一种方便的方式来上传单个测试用例，并通过特定的目标进行测试。 最常见的用例是测试由外部研究人员报告或由其他系统发现的错误。

## Jobs
This page provides a way to create new or modify existing job configurations.

该页面提供了一种创建新作业或修改现有作业配置的方法。

## Configuration
This is an administrative page with a variety of settings including ClusterFuzz
access control, credentials, etc. This page is available to admin users only.

这是一个具有各种设置的管理页面，包括ClusterFuzz访问控制，凭据等。该页面仅对管理员用户可用。