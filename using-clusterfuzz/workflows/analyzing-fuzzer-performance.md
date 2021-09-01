---
layout: default
title: Analyzing fuzzer performance
permalink: /using-clusterfuzz/workflows/analyzing-fuzzing-performance/
nav_order: 3
parent: Workflows
grand_parent: Using ClusterFuzz
---

# Analyzing fuzzer performance

ClusterFuzz 尽可能地将fuzzing处理自动化，但用户有责任编写和维护能够发现安全漏洞的fuzzers。本页面
提供了关于如何分析运行在 ClusterFuzz 上的fuzzer性能的建议。

**注意**：本页面只适用于以[fuzz targets]做[coverage guided]的[libFuzzer]或[AFL]进行fuzzing。

[AFL]: http://lcamtuf.coredump.cx/afl/
[libFuzzer]: https://llvm.org/docs/LibFuzzer.html
[coverage guided]: {{ site.baseurl }}/reference/coverage-guided-vs-blackbox/
[fuzz targets]: {{ site.baseurl }}/reference/glossary/#fuzz-target

- TOC
{:toc}

---

## When to analyze fuzz target performance

定期监测fuzz targets的性能是很重要的，尤其是在创建了新的target后。如果一个target发现了许多新的crashes，修复它们可能比分析性能更重要。但如果一个target在一段时间内没有发现任何crash，你可能应该检查它的性能。

## Performance factors

* **速度**对于fuzzing至关重要。在fuzzzing过程中，target产生测试用例的速度越快越好。
* **代码覆盖率**应该随着时间的推移而增长。一个fuzz target应该不断产生新的 "有趣的 "测试用例，以执行目标程序的各个部分。
* **阻塞问题**应得到解决。如果一个fuzz target经常报告超时、内存不足或其他崩溃，那么它就会阻碍target找到更多有趣的问题。

## Fuzzer stats

*Fuzzer stats*页面提供了关于fuzzer性能的指标。使用页面上的过滤器，您可以看到这些指标（如执行速度或崩溃次数）随时间（按天数分组）的变化。你可以使用 "按fuzzer分组 "将不同的fuzzer相互比较。也有
还有一个 "按时间分组 "的过滤器，以图表形式显示fuzzer的统计数据，而不是原始数字。

这个功能需要一个[生产设置]({{ site.baseurl }}/production-setup/)，因为fuzzer的统计数据存储在[BigQuery]中。统计数据通常会延迟24小时，因为数据每天会上传到BigQuery。

[BigQuery]: https://cloud.google.com/bigquery/

## Performance report

ClusterFuzz提供了自动生成的性能报告，以识别性能问题，并就如何解决这些问题提出建议。报告还对问题进行了优先排序，并提供了证明问题的fuzzer日志。

## Coverage report

代码覆盖率是评估fuzzer性能的一个非常重要的指标。 看一下代码覆盖率报告，你可以准确看到目标程序的哪些部分被fuzz，哪些部分从未被执行。如果你为ClusterFuzz设置了一个[code coverage builder]，你可以在Fuzzer统计中找到覆盖率的链接报告。否则，你可以在本地生成代码覆盖率报告。对于用C和C++编写的targets，我们推荐使用[基于Clang的代码覆盖]。

[code coverage builder]: {{ site.baseurl }}/using-clusterfuzz/advanced/code-coverage/

## Fuzzer logs

如果以上都不能给你提供足够的关于fuzzer性能的信息。 查看fuzzer的日志可能会有帮助。在fuzzer统计页面，你可以找到一个链接到存储日志的GCS bucket。你也可以手动导航到谷歌云存储[web界面]。

[基于Clang的代码覆盖]: https://clang.llvm.org/docs/SourceBasedCodeCoverage.html
[web界面]: https://console.cloud.google.com/storage/browser
