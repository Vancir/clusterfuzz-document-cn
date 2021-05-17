---
layout: default
title: Coverage guided vs blackbox fuzzing
parent: Reference
nav_order: 2
permalink: /reference/coverage-guided-vs-blackbox/
---

# Coverage guided vs blackbox fuzzing

ClusterFuzz支持两种模糊测试类型: 覆盖率导向的模糊测试(使用[libFuzzer]和[AFL])以及黑盒模糊测试.

---

## Coverage guided fuzzing

覆盖率导向的模糊测试(也称为灰盒模糊测试)是指通过对程序进行插装来跟踪提供给[Fuzz目标]的各个输入的代码覆盖率. [模糊测试引擎]通过该信息来决定对哪些输入进行变异才能使覆盖率最大化.

对于每个目标, 模糊测试引擎都会构建输入的[语料库]. 随着引擎通过突变产生新的输入, 程序的覆盖率会随着时间的推移而增长. 

ClusterFuzz支持两种模糊测试引擎: [libFuzzer] (推荐)以及[AFL]. 

### When should I use coverage guided fuzzing?

建议使用覆盖率导向的模糊测试, 因为它通常是最有效的的方式. 其在以下情况中效果最佳:
- 目标是独立的. 
- 目标是确定性的. 
- 目标每秒可以执行数十次或更多次(理想情况下为数百次或更多).

例如, 二进制格式(例如图像格式)解析器非常适合于此种方法. 

## Blackbox fuzzing

黑盒Fuzzer会在不了解目标内部行为或实现的情况下为目标程序生成输入. 


黑盒Fuzzer可能会从头开始生成输入, 或者依赖于有效输入文件的静态语料库作为突变的基础. 与覆盖率导向的模糊测试不同的是它的语料库并不会增长.

### When should I use blackbox fuzzing?

在以下情况下, 黑匣子模糊测试效果更好:
- 目标体积很大. 
- 对于相同的输入, 输出结果不定. 
- 目标重复执行很慢.
- 输入格式复杂或结构化程度高(例如, JavaScript之类的编程语言).

例如, 浏览器的DOM Fuzzer可能会针对诸如Chrome之类的目标生成HTML输入运行, 但没有任何覆盖率反馈来指导其变异. 

[AFL]: http://lcamtuf.coredump.cx/afl/
[libFuzzer]: https://llvm.org/docs/LibFuzzer.html
[corpus]: {{ site.baseurl }}/reference/glossary/#corpus
[Fuzz目标]: {{ site.baseurl }}/reference/glossary/#fuzz-target
[模糊测试引擎]: {{ site.baseurl }}/reference/glossary/#fuzzing-engine
