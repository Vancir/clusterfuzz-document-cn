---
layout: default
title: libFuzzer and AFL++
parent: Setting up fuzzing
nav_order: 1
permalink: /setting-up-fuzzing/libfuzzer-and-afl/
---

# libFuzzer and AFL

本页将引导您使用[libFuzzer]或[AFL]运行[覆盖率导向的模糊测试], 同时还为其他高阶功能(例如词典和种子语料库)提供参考. 

- TOC
{:toc}

---

## Prerequisites

### Compiler
LibFuzzer和AFL均需要使用Clang编译器进行代码插装. 在本文档中, 我们采用的版本为Clang **6.0**及更高. 但为了更好的使用ClusterFuzz, 我们建议使用Clang最新稳定版本. 要获得Clang最新稳定版本，您可以从[快照页面]（Windows）下载它，或按照[apt页] (Ubuntu / Debian)上的说明进行操作。 否则，您可以从[发布页面]下载Clang发行版，或使用包管理器安装一个Clang发行版。 我们将在示例中将这些编译器称为`$CC`和`$CXX`。 在环境中进行设置，以便您可以复制并粘贴示例命令：

```bash
export CC=/path/to/clang
export CXX=/path/to/clang++
```

[发布页面]: http://releases.llvm.org/download.html
[apt页]: https://apt.llvm.org/
[快照页面]: https://llvm.org/builds/

### Platform

Linux, macOS和Windows均支持libFuzzer. 对于Windows平台, 您需要在cmd.exe中运行命令, 并且需要**9.0**或更高版本的Clang支持, 您可以从[LLVM Snapshot Builds页面]下载合适的版本. 

AFL则仅支持在Linux平台上运行.

[LLVM Snapshot Builds页面]: https://llvm.org/builds/

## Builds

### libFuzzer

LibFuzzer的目标十分容易进行构建. 只需要通过选项`-fsanitize=fuzzer`编译链接[fuzz target]和例如[AddressSanitizer]这样的[sanitizer] (`-fsanitize=address`)即可. 

```bash
$CXX -fsanitize=address,fuzzer fuzzer.cc -o fuzzer
# 通过运行来测试构建的Fuzzer
./fuzzer -runs=10
# 将Fuzzer打包上传到ClusterFuzz.
zip fuzzer-build.zip fuzzer
```

libFuzzer构建包是包含您要Fuzz的目标及其依赖项的Zip归档文件.

[sanitizer]: {{ site.baseurl }}/reference/glossary/#sanitizer

### AFL
ClusterFuzz支持使用AFL ++来fuzzing libFuzzer的harness功能（`LLVMFuzzerTestOneInput`）。 AFL ++必须与[AddressSanitizer]一起使用。
要为AFL构建fuzz目标，请运行我们的[script]，该脚本下载并构建AFL和`FuzzingEngine.a`，您可以将目标链接到该库以使其与AFL兼容。 然后使用-fsanitize-coverage = trace-pc-guard和-fsanitize = address编译并链接目标。

注意:由于将不会启用CMPLOG和COMPCOV之类的高级模糊功能，因此不会充分利用AFL ++。
因此，建议使用oss-fuzz代替创建（多个）模糊测试软件包，因为每个软件包都带有随机选项。


```bash
# Build afl-fuzz and FuzzingEngine.a 生成afl-fuzz和FuzzingEngine.a
./build_afl.bash
# Compile target using ASan, coverage instrumentation, and link against FuzzingEngine.a 使用ASan，coverage工具和针对FuzzingEngine.a的链接来编译目标
$CXX -fsanitize=address -fsanitize-coverage=trace-pc-guard fuzzer.cc FuzzingEngine.a -o fuzzer
# Test out the build by fuzzing it. INPUT_CORPUS is a directory containing files. Ctrl-C when done. 通过模糊测试构建。 INPUT_CORPUS是包含文件的目录。 完成后，请按Ctrl-C。
AFL_SKIP_CPUFREQ=1 ./afl-fuzz -i $INPUT_CORPUS -o output -m none ./fuzzer
# Create a fuzzer build to upload to ClusterFuzz. 创建一个模糊器构建以上载到ClusterFuzz。
zip fuzzer-build.zip fuzzer afl-fuzz afl-showmap
```

AFL构建是zip文件，其中包含您要fuzz的任何目标，它们的依赖关系以及AFL的依赖关系：`afl-fuzz`和`afl-showmap`（均由[script]构建）。

## Creating a job type
LibFuzzer作业**必须**在名称中包含字符串“**libfuzzer**”，AFL ++作业**必须**在名称中包含字符串“**afl**”。 作业还必须包含他们正在使用的sanitizer的名称（例如“ asan”，“ msan”或“ ubsan”）。
“**libfuzzer_asan_my_project**” 和“**afl_asan_my_project**” 是使用AddressSanitizer的libFuzzer和AFL作业的正确名称示例。

要为libFuzzer或AFL创建作业：
1. 导航到“工作”页面。
2. 转到“添加新工作”表格。
3. 填写“名称”和“平台”（LINUX）。
4. 在“选择/修改fuzzer”字段中启用所需的fuzzer，例如**libFuzzer**，**honggfuzz**或**afl**。
5. 如果要设置 **AFL** 作业，请使用模板“**afl**” 和“**engine_asan**”。
6. 如果要设置“**honggfuzz**”作业，请使用模板“ **honggfuzz**” 和“**engine_asan**”。
7. 如果要设置**libFuzzer**作业，请根据所使用的sanitizer使用模板“**libfuzzer**” 和“**engine_$SANITIZER**”（例如“**engine_asan**” ）。
8. 选择您的构建（您的zip文件包含目标二进制文件）以作为“自定义构建”上传。 如果要在生产环境中运行ClusterFuzz，建议设置[构建管道]并按照[这些]说明提供连续的构建，而不要使用“自定义构建”。
9. 使用“添加”按钮将作业添加到ClusterFuzz。

[这些]: {{ site.baseurl }}/production-setup/setting-up-fuzzing-job/

### Enabling corpus pruning
启用[语料库修剪]每天运行一次，以防止不受控制的语料库增长，这一点很重要。 必须通过在libFuzzer ASan作业的“**环境字符串**”中设置`CORPUS_PRUNE = True`来完成此操作。

## Checking results
您可以通过查看[bot日志]来观察ClusterFuzz对构建进行模糊测试。 它发现的任何错误都可以在*Testcases*页面上找到。 如果在生产环境中运行ClusterFuzz（即不在本地），则还可以查看[崩溃统计信息]和[fuzzer统计信息]（通常需要等待一天才能查看fuzzer统计信息）。

## Seed corpus
您可以选择在构建中上传一个zip文件，其中包含用于ClusterFuzz的示例输入，以提供给您的Fuzzer。 我们称其为种子语料库。 对于给定的fuzz目标，如果满足以下条件，ClusterFuzz将使用文件作为种子语料库：

* 它与fuzz目标位于构建的同一目录中。
* 它与fuzz目标具有相同的名称（不包括.exe扩展名），后跟_seed_corpus.zip（即<fuzz_target>的<fuzz_target> _seed_corpus.zip）。

我们建议在构建时压缩有趣输入的目录，以创建种子语料库。

## Dictionaries
ClusterFuzz支持使用[libFuzzer/AFL Dictionaries]。 字典是AFL或libFuzzer在模糊过程中可以插入标记的列表。 对于给定的fuzz目标，如果满足以下条件，ClusterFuzz将使用文件作为字典：

* 它与模糊目标位于构建的同一目录中。
* 它与模糊目标具有相同的名称（不包括.exe扩展名），后跟.dict（即<fuzz_target>的<fuzz_target> .dict）。

[libFuzzer/AFL Dictionaries]: https://llvm.org/docs/LibFuzzer.html#dictionaries

## AFL limitations
尽管ClusterFuzz支持AFL进行模糊测试，但不支持将其用于[语料库修剪]和[崩溃最小化]。 因此，如果使用AFL，则还应该使用支持这些任务的libFuzzer。

[AFL++]: https://github.com/AFLplusplus/AFLplusplus
[AddressSanitizer]: https://clang.llvm.org/docs/AddressSanitizer.html
[afl_driver.cpp]: https://raw.githubusercontent.com/llvm-mirror/compiler-rt/master/lib/fuzzer/afl/afl_driver.cpp
[bot日志]: {{ site.baseurl }}/getting-started/local-instance/#viewing-logs
[构建管道]: {{ site.baseurl }}/production-setup/build-pipeline/
[语料库修剪]: {{ site.baseurl }}/reference/glossary/#corpus-pruning
[覆盖率导向的模糊测试]: {{ site.baseurl }}/reference/coverage-guided-vs-blackbox/#coverage-guided-fuzzing
[崩溃最小化]: {{ site.baseurl }}/reference/glossary/#minimization
[崩溃统计信息]: {{ site.baseurl }}/using-clusterfuzz/ui-overview/#crash-statistics
[fuzz target]:https://llvm.org/docs/LibFuzzer.html#id22
[fuzzer统计信息]: {{ site.baseurl }}/using-clusterfuzz/ui-overview/#fuzzer-statistics
[latest AFL source]:http://lcamtuf.coredump.cx/afl/releases/afl-latest.tgz
[libFuzzer]: https://llvm.org/docs/LibFuzzer.html
[script]: {{ site.baseurl }}/setting-up-fuzzing/build_afl.bash
