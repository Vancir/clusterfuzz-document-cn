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
If you follow this tutorial using [local ClusterFuzz] server and bot instances,
and you do not have any other fuzzing tasks running, you should see the string:
`fuzz libFuzzer libfuzzer_asan_linux_openssl` show up in the [bot logs]. This
means that ClusterFuzz is fuzzing your build. Soon after that you should see a
stack trace and the string: `AddressSanitizer: heap-buffer-overflow` in the log.

If you follow this tutorial using a [production instance of ClusterFuzz], you
should be able to see the string `fuzz libFuzzer libfuzzer_asan_linux_openssl`
on the *Bots* page. The timing also depends on the other workload you may have.

Some time later, you can go to the ClusterFuzz homepage (ie: the *Testcases*
page) and you will see a testcase titled **"Heap-buffer-overflow READ{\*}"**.
This is the Heartbleed vulnerability found by ClusterFuzz.

[bot logs]: {{ site.baseurl }}/getting-started/local-instance/#viewing-logs
[local ClusterFuzz]: {{ site.baseurl }}/getting-started/local-instance/
[production instance of ClusterFuzz]: {{ site.baseurl }}/production-setup/clusterfuzz/
