---
layout: default
title: Triaging new crashes
permalink: /using-clusterfuzz/workflows/triaging-new-crashes/
nav_order: 1
parent: Workflows
grand_parent: Using ClusterFuzz
---

# Triaging new crashes

ClusterFuzz的建立是为了尽可能地减轻fuzz中的人工工作。特别是bug分类，一直是一个困难和需要手动处理的过程。

- TOC
{:toc}

---

## Filtering testcases

ClusterFuzz对每个测试案例进行分析。比如，ClusterFuzz可以通过[crash类型]来确定一个crash是否有影响。

*测试案例 *页面提供对测试案例的过滤和搜索功能。

你可以通过以下几种方式过滤：

- [crash类型]
- [crash状态]
- fuzzer 名字
- [job] 名字
- [稳定性] 复现crash
- 安全影响

默认情况下，只有享有特权的用户才可以看到具有安全影响的问题。可以在不泄露敏感信息的情况下向一些用户授予访问权。参见[访问控制]，给予更细化的访问。

[crash状态]: {{ site.baseurl }}/reference/glossary/#crash-state
[crash类型]: {{ site.baseurl }}/reference/glossary/#crash-type
[job]: {{ site.baseurl }}/reference/glossary/#job
[稳定性]: {{ site.baseurl }}/reference/glossary/#reliability-of-reproduction

## Regression revision range

虽然只适用于[稳定复现]crashes,但是[回归测试范围]往往是最有用的分类信息。它显示了该问题被引入的提交范围。

[构建版本]的频率越高，范围就越窄。如果你归档了你构建的每一个版本，ClusterFuzz就可以准确指出引入错误的commit。

[构建版本]: {{ site.baseurl }}/production-setup/build-pipeline/
[回归测试范围]: {{ site.baseurl }}/reference/glossary/#regression-range
[稳定复现]: {{ site.baseurl }}/reference/glossary/#reliability-of-reproduction

## Crash stacktrace

当使用[sanitizers]时，bug和崩溃栈信息有关，可以帮助你确定崩溃的原因。这更像是一门艺术，而不是一门精确的科学，没有一个过程可以遵循，利用这些信息找到一个罪魁祸首的变化列表。

也就是说，在回归范围内寻找更改崩溃栈信息中所看到的相同文件的变化（如果有的话），往往是有帮助的。否则，"git blame "或类似的工具可能有助于找到一个更熟悉相关代码的开发者。这个开发者可能是一个很好的第一联络点。

[sanitizers]: {{ site.baseurl }}//reference/glossary/#sanitizer

## Crash statistics

ClusterFuzz还提供了关于问题发生频率和条件的[统计]。例如，ClusterFuzz会跟踪问题在哪些平台上重现
以及哪些模糊器发现了这些问题。

这些有时会对其他方法失败的bugs提供有用的信息。例如，如果不能直接修改崩溃栈中的一个文件，但你发现它只在一个特定的平台上重现，那么该平台上的一个大的行为变化可能是造成crash的原因。对于一个无法重现的crash，知道它在3天前开始发生，与一个危险的变化同时发生，可能会很有用。

[访问控制]: {{ site.baseurl }}/using-clusterfuzz/advanced/access-control/
[统计]: {{ site.baseurl }}/using-clusterfuzz/ui-overview/#crash-statistics
