---
layout: default
title: Glossary
parent: Reference
nav_order: 1
permalink: /reference/glossary/
---

# Glossary

本页面提供一份术语表, 描述各个术语在ClusterFuzz情境中的具体含义. 

- TOC
{:toc}
---

## Bot

一个运行ClusterFuzz[任务](#task)的机器. 

## Corpus

[Fuzz目标](#fuzz-target)的一组输入. 在大多数情况下, 它指一组生成最大代码覆盖率的最小测试输入. 

## Corpus pruning

一项任务, 在保持相同代码覆盖率的同时, 选取一个[语料库](#corpus)并除去不必要的输入. 

## Crash state

我们从崩溃堆栈跟踪记录里生成的一项用于删除重复数据的特征签名. 

## Crash type

崩溃的类型. ClusterFuzz使用崩溃类型来确认崩溃的严重程度. 

对于安全漏洞, 类型可能是(但不限于)以下:
- Bad-cast
- Heap-buffer-overflow
- Heap-double-free
- Heap-use-after-free
- Stack-buffer-overflow
- Stack-use-after-return
- Use-after-poison

其他崩溃类型包括: 
- Null-dereference
- Timeout
- Out-of-memory
- Stack-overflow
- ASSERT

## Fuzz target

一个函数或程序, 接受一串字节数组并使用被测API对这些字节进行了某些研究人员感兴趣的操作. 有关更多详细说明请参见[libFuzzer documentation](https://llvm.org/docs/LibFuzzer.html#fuzz-target). 通常由[libFuzzer]或[AFL]为Fuzz目标提供字节数组, 以进行覆盖率导向的模糊测试.

## Fuzzer

一个能够为测试目标软件而生成/变异特定格式输入的程序. 例如, 它可能是一个通过生成有效的JavaScript测试用例来对JavaScript引擎(比如V8)进行模糊测试的程序. 

## Fuzzing engine

用于执行覆盖率导向的模糊测试的工具. 模糊测试引擎通常会根据新的覆盖率信息对输入进行变异, 获取覆盖率信息并将输入添加到语料库中去. ClusterFuzz目前支持的模糊测试引擎有[libFuzzer]和[AFL]. 有关配置[libFuzzer]和[AFL]的更多详细说明请参见我们的[这篇教程]({{site.baseurl }}/setting-up-fuzzing/libfuzzer-and-afl/). 

## Job type

有关如何运行特定目标程序进行模糊测试以及如何定位构建位置的规范, 由一系列环境变量值组成. 

## Minimization

一项试图将[测试用例](#testcase)尺寸尽可能最小化的同时依然能在目标程序上触发相同错误的[任务](#task). 

## Reliability of reproduction

如果目标程序对于给定的输入始终以相同的[崩溃状态](#crash-state)崩溃, 则称为可靠地重现崩溃. 


## Regression range

最初引入该错误的一系列提交. 它的格式为`x:y`, 其中:
* x是起始[修订版](#revision)(含).
* y是最终[修订版](#revision)(不包含).

## Revision

一个可以用来标识特定构建的数字(不是git哈希). 每个源代码修订版该数值都会增加. 对于SVN该数值可能只是SVN源码修订版, 但对于Git, 您则需要创建一个数字到git哈希的等效映射. 映射编号可以是以`1`开头并递增的ID, 也可以是日期格式(例如`20190110`). 

## Sanitizer

一种[动态测试](https://en.wikipedia.org/wiki/Dynamic_testing)工具, 使用编译时插装来检测程序运行期间的错误. 

例如:
* [ASan] (aka AddressSanitizer)
* [LSan](https://clang.llvm.org/docs/LeakSanitizer.html) (aka LeakSanitizer)
* [MSan](https://clang.llvm.org/docs/MemorySanitizer.html) (aka MemorySanitizer)
* [UBSan](https://clang.llvm.org/docs/UndefinedBehaviorSanitizer.html)
  (aka UndefinedBehaviorSanitizer)
* [TSan](https://clang.llvm.org/docs/ThreadSanitizer.html) (aka ThreadSanitizer)

[Clang]编译器对Sanitizers的支持最佳. [ASan], 全称为AddressSanitizer, 通常是最重要的sanitizer, 因为它发现来最多的内存破坏错误. 

## Task

由[bot](#bot)执行的工作单元, 例如模糊测试会话或测试用例最小化. 

## Testcase

导致目标程序崩溃或错误的输入. 在测试用例详细信息页面上, 您可以下载"Minimized Testcase"或"Unminimized Testcase", 这些指的均是需要传递给目标程序的输入.  

[ASan]: https://clang.llvm.org/docs/AddressSanitizer.html
[libFuzzer]: https://llvm.org/docs/LibFuzzer.html
[AFL]: http://lcamtuf.coredump.cx/afl/
[Clang]: https://clang.llvm.org/
