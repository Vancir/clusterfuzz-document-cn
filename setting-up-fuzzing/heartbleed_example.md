---
layout: default
title: Heartbleed example
parent: Setting up fuzzing
nav_order: 3
permalink: /setting-up-fuzzing/heartbleed-example/
---

# Finding Heartbleed

本教程将会向您展示如何使用libFuzzer和Clusterfuzz挖掘[Heartbleed]漏洞.

- TOC
{:toc}
---

## Prerequisites

假设您现在正在使用一台Linux bot. 
查阅位于[libFuzzer和AFL++]文档里的[编译器段落], 获取以下示例的可用编译器, 并确保设置好了`CC`和`CXX`环境变量. 

[Heartbleed]: https://en.wikipedia.org/wiki/Heartbleed
[Getting Started]: {{ site.baseurl }}/getting-started/
[libFuzzer和AFL++]: {{ site.baseurl }}/setting-up-fuzzing/libfuzzer-and-afl/
[编译器段落]: {{ site.baseurl }}/setting-up-fuzzing/libfuzzer-and-afl/#compiler

## Building a libFuzzer target for OpenSSL

运行以下命令构建一个用于OpenSSL的libFuzzer target. 

[comment]: <> (TODO(metzman): Check that the URLs work when repo gets published.)
```bash
# 下载和解压一份受漏洞影响的OpenSSL版本代码
curl -O https://ftp.openssl.org/source/old/1.0.1/openssl-1.0.1f.tar.gz
tar xf openssl-1.0.1f.tar.gz

# 构建带有ASan和fuzzer插装的OpenSSL
cd openssl-1.0.1f/
./config

# $CC必须指向clang二进制文件, 如何设置请查看上述"编译器段落"链接
make CC="$CC -g -fsanitize=address,fuzzer-no-link"
cd ..

# 下载fuzz target及其依赖的数据. 
curl -O https://raw.githubusercontent.com/google/clusterfuzz/master/docs/setting-up-fuzzing/heartbleed/handshake-fuzzer.cc
curl -O https://raw.githubusercontent.com/google/clusterfuzz/master/docs/setting-up-fuzzing/heartbleed/server.key
curl -O https://raw.githubusercontent.com/google/clusterfuzz/master/docs/setting-up-fuzzing/heartbleed/server.pem

# 构建用于ClusterFuzz的OpenSSL fuzz target ($CXX需要指向clang++二进制文件):
$CXX -g handshake-fuzzer.cc -fsanitize=address,fuzzer openssl-1.0.1f/libssl.a \
  openssl-1.0.1f/libcrypto.a -std=c++17 -Iopenssl-1.0.1f/include/ -lstdc++fs   \
  -ldl -lstdc++ -o handshake-fuzzer

zip openssl-fuzzer-build.zip handshake-fuzzer server.key server.pem
```

## Uploading the fuzzer to ClusterFuzz

首先我们需要创建一个任务: 

* 导航至*Jobs*页面.
* 来到"ADD NEW JOB"表单处.
* 依照以下内容填充任务信息:
    * 填写**"libfuzzer_asan_linux_openssl"** 至 "Name".
    * 填写**"LINUX"** 至 "Platform".
    * 填写**"libFuzzer"** 至 "Select/modify fuzzers".
    * 填写**"libfuzzer"** 和 **"engine_asan"** 至 "Templates".
    * 填写`CORPUS_PRUNE = True` 至 "Environment String".
* 选择 openssl-fuzzer-build.zip并上传为"Custom Build".
* 使用"ADD"按钮将该Job添加至ClusterFuzz.

## Fuzzing and seeing results

如果您遵照[local ClusterFuzz]教程配置好了本地的server和bot实例, 并且同时也没有其他任何fuzzing任务正在运行, 那么您接下来应该就能在[bot logs]处看到`fuzz libFuzzer libfuzzer_asan_linux_openssl`的字样. 这代表着ClusterFuzz正在对您构建的代码进行模糊测试. 不久后您就能在日志里看到崩溃栈信息以及字符串`AddressSanitizer: heap-buffer-overflow`. 

如果您遵照的是教程[production instance of ClusterFuzz], 那么您可以在*Bots*页面看到`fuzz libFuzzer libfuzzer_asan_linux_openssl`字样. 具体花费的时间取决于您可能承担的其他工作负荷. 

随后, 您可以转到ClusterFuzz首页(或者*Testcases*页面), 您将会看到一个标题名为**"Heap-buffer-overflow READ{\*}"**的测试用例. 这就是由ClusterFuzz发现的心脏滴血漏洞. 

[bot logs]: {{ site.baseurl }}/getting-started/local-instance/#viewing-logs
[local ClusterFuzz]: {{ site.baseurl }}/getting-started/local-instance/
[production instance of ClusterFuzz]: {{ site.baseurl }}/production-setup/clusterfuzz/
