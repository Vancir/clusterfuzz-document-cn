---
layout: default
title: Fixing a bug
permalink: /using-clusterfuzz/workflows/fixing-a-bug/
nav_order: 2
parent: Workflows
grand_parent: Using ClusterFuzz
---

# Fixing a bug

如果能帮助修复错误，发现错误是非常有用的。为了使修复错误更加容易，ClusterFuzz提供了必要的信息来重现问题，并确认修复是正确的。

本文档重点关注测试用例的详细页面，因为它包含了修复错误的大部分相关信息。

- TOC
{:toc}

---

## Reproducing an issue

报告的*概述*部分包含了触发该问题下载的测试用例和构建的链接。

在崩溃栈回溯的顶部（可能需要扩展），ClusterFuzz显示了命令行和最初发生crash时设置的重要环境变量。这些环境变量可能是复现crash的必要条件。

## Fixed testing

每重新运行一次，ClusterFuzz都会针对最新的版本重新运行测试用例，直到它检测到这些用例被修复。当一个问题被检测到为修复状态，该问题的修复状态将被更新为 "是"，并且*修复范围*部分将链接到包含该修复的提交范围。

如果测试用例与某个bug相关联，ClusterFuzz也会留下记录并更改问题状态，以表明它已经验证了修复的正确性
。

## Unreliable crashes

ClusterFuzz认为那些没有[稳定复现]的测试用例不重要。但是，如果一个[crash状态]经常出现，尽管没有一个稳定的测试用例，ClusterFuzz将为其提交一个bug。

当ClusterFuzz发现同一[crash状态]下的可以稳定复现的测试用例时，它会创建一个新的报告，并删除含有不稳定测试用例的旧报告。在这种情况下，建议等待几天，看看是否能找到可复现的测试案例。

如果没有找到，可以尝试多次运行测试用例，看是否能在本地得到一个一致的crash。如果失败了，我们建议根据crash的栈回溯推送一个推测性修复，然后让ClusterFuzz来验证crash是否在接下来的几天内停止发生。

如果提交了一个跟踪bug，ClusterFuzz将每天自动查看[crash统计]，如果该crash不再频繁发生（即在*2周内没有看到crash*），则将该bug状态设置为Verified并关闭该Bug。如果没有提交bug，那么如果在一周内没有看到不可复现的测试用例，就会自动关闭。

[crash统计]: {{ site.baseurl }}/using-clusterfuzz/ui-overview/#crash-statistics
[crash状态]: {{ site.baseurl }}/reference/glossary/#crash-state
[稳定复现]: {{ site.baseurl }}/reference/glossary/#reliability-of-reproduction
