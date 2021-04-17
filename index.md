---
layout: default
title: ClusterFuzz
permalink: /
nav_order: 1
has_children: true
---

# ClusterFuzz

<p align="center">
  <img src="{{ site.baseurl }}/images/logo.png" width="400">
</p>

ClusterFuzz是一个可扩展的用于发现软件中安全性和稳定性问题的[模糊测试](https://en.wikipedia.org/wiki/Fuzzing)基础设施. 

Google使用ClusterFuzz对所有的Google产品进行模糊测试, 并且作为模糊测试后端为[OSS-Fuzz]提供支持. 

ClusterFuzz提供了许多功能, 并且能将模糊测试功能无缝地集成到软件项目的开发过程中：

- 高可扩展性. 可以在任何大小的群集上运行(例如Google运行了约30,000个虚拟机实例).
- 准确地对产生的崩溃进行去重.
- 支持使用各种问题跟踪工具(例如[Monorail], [Jira]）自动地对错误进归档, 分类和关闭.
- 支持使用多种[覆盖率导向的模糊测试引擎]()([libFuzzer], [AFL++]和[Honggfuzz])以获得最佳结果.(结合[Ensemble模糊测试]()和[模糊测试策略]()).
- 支持[黑盒模糊测试]().
- 测试用例最小化. 
- 通过[二分选择]()进行回归查找.
- 提供了分析Fuzzer性能和崩溃率的统计信息.
- 易于使用的Web界面, 用于管理和查看崩溃信息.
- 基于[Firebase]()可支持各种身份验证提供程序. 

[Monorail]: https://opensource.google.com/projects/monorail

## Trophies

截至2020年9月, ClusterFuzz为Google发现了25,000多个错误(例如[Chrome]), 并在超过[340]个集成到[OSS-Fuzz]的开源项目中发现了[~22,500]个错误. 

[Chrome]: https://bugs.chromium.org/p/chromium/issues/list?can=1&q=label%3AClusterFuzz+-status%3AWontFix%2CDuplicate
[~22,500]: https://bugs.chromium.org/p/oss-fuzz/issues/list?q=-status%3AWontFix%2CDuplicate%20-component%3AInfra&can=1
[340]: https://github.com/google/oss-fuzz/tree/master/projects
[OSS-Fuzz]: https://github.com/google/oss-fuzz
[Monorail]: https://opensource.google.com/projects/monorail
[Jira]: https://www.atlassian.com/software/jira
[bisection]: https://en.wikipedia.org/wiki/Bisection_(software_engineering)
[Firebase]: https://firebase.google.com/docs/auth
[libFuzzer]: http://llvm.org/docs/LibFuzzer.html
[AFL++]: https://github.com/AFLplusplus/AFLplusplus
[Honggfuzz]: https://github.com/google/honggfuzz
[blackbox fuzzing]: {{ site.baseurl }}/setting-up-fuzzing/blackbox-fuzzing/
[coverage guided fuzzing engines]: {{ site.baseurl }}/setting-up-fuzzing/libfuzzer-and-afl/
[fuzzing strategies]: https://i.blackhat.com/eu-19/Wednesday/eu-19-Arya-ClusterFuzz-Fuzzing-At-Google-Scale.pdf#page=27
[ensemble fuzzing]: https://www.usenix.org/system/files/sec19-chen-yuanliang.pdf
