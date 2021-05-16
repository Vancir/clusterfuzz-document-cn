---
layout: default
title: Blackbox fuzzing
parent: Setting up fuzzing
nav_order: 2
permalink: /setting-up-fuzzing/blackbox-fuzzing/
---

# Blackbox fuzzing

本页面将引导您设置第一个[黑盒fuzzer]. 

- TOC
{:toc}
---

## Providing builds

开始模糊测试之前, 您需要使用[sanitizer]构建目标代码. [AddressSanitizer](https://clang.llvm.org/docs/AddressSanitizer.html)是目前最常用于模糊测试的的[sanitizer], 它通过代码插装来检查内存安全性问题. Clang是其支持的编译器, GCC应该也受支持, 选用那个编译器则具体取决于项目的构建系统. 简单来说, 就是需要将`-fsanitize=address`标志添加到您的C/C++编译器和链接器标志中去.


## Creating a job type

[作业]的类型是用于描述如何运行特定目标程序进行模糊测试的规范. 它由环境变量值组成. 

[作业]的名称非常**重要**, 并且必须要包含 **"asan"**, **"msan"**, **"ubsan"**或 **"tsan"** 关键字, 具体取决于您使用的[sanitizer]. 比如一个有效名称可以是 **"asan_linux_app"**. 

一个基本的二进制 **"app"** 作业类型可能类似于以下这样:

```
APP_NAME = app
APP_ARGS = --some_interesting_option --some_very_important_option
REQUIRED_APP_ARGS = --some_very_important_option
CUSTOM_BINARY = True
TEST_TIMEOUT = 30
```

拆分来看,
1. `APP_NAME`表示目标程序的名称. 
2. `APP_ARGS`是要传递给目标应用程序的参数. 此变量应包含您想要传递的必需参数和可选参数.
3. `REQUIRED_APP_ARGS`是指必须传递给目标应用程序的参数, 并且这些参数将始终被使用. 而其他在`APP_ARGS`中指定的参数以及未在此处指定的参数, 如果在最小化测试用例过程中发现该参数并不会影响崩溃的重新, 那么就会被ClusterFuzz进行移除. 
4. `CUSTOM_BINARY`指是否使用用户上传的归档文件, 而不是从GCS存储桶中拉取文件. 
5. `TEST_TIMEOUT`是每个测试用例的最大超时时间(以秒为单位). 

要创建一个作业: 
1. 导航到*Jobs*页面. 
2. 转到"ADD NEW JOB"表单. 
3. 填写表单里的"Name"和"Platform".
4. 选择您构建好的程序(以zip进行打包, 同时zip包内还需要包含待测试的目标二进制文件)并作为"Custom Build"上传. 
5. 使用"ADD"按钮将作业添加到ClusterFuzz. 

## Uploading a fuzzer

ClusterFuzz中的[黑盒fuzzer]是指一个程序, 它接受语料库作为输入, 并将变异或生成的测试用例输出到输出目录. 该程序必须采用以下命名参数: 

1. `--input_dir <directory>`. 这里指定输入目录, 目录包含给定Fuzzer的语料库. 
2. `--output_dir <directory>`. 这里指定输出目录, 目录包含Fuzzer的写入数据.
3. `--no_of_files <n>`. 这里会限制Fuzzer向输出目录写入测试用例的数量. 
   
由Fuzzer生成的测试用例文件名必须以前缀`fuzz-`进行命名. 这有助于ClusterFuzz知道哪些是需要进行模糊测试的的文件. 输出目录还必须包含执行测试用例所需的所有依赖项. 

该Fuzzer的执行入口应该是一个以`run`开头命名的文件. 例如, 使用Python编写的Fuzzer可能名为`run.py`.

要将其上传到ClusterFuzz, 请将Fuzzer连同其依赖项一起打包到一个zip压缩包内, 然后：


1. 导航到*Fuzzers*页面。
2. 点击"CREATE NEW"按钮. 
3. 填写Fuzzer的"Name". 
4. 选择要上传的Fuzzer压缩包. 
5. 点击"Select/modify jobs". 
6. 标记上面创建的作业. 
7. 点击"SUBMIT"

## Checking results

Bots需要花一些时间来启动您上传的Fuzzer. 您可以通过*Bots*页面来检查哪些Bot正在运行您的Fuzzer. 一旦Bot运行完您的模糊器, 您就可以访问*Fuzzers*页面(请参阅第二列)查看运行过程的控制台输出和测试用例. 您也可以检查[Bot日志].

[黑盒fuzzer]: {{ site.baseurl }}/reference/coverage-guided-vs-blackbox/#blackbox-fuzzing
[fuzzer]: {{ site.baseurl }}/reference/glossary/#fuzzer
[作业]: {{ site.baseurl }}/reference/glossary/#job-type
[sanitizer]: {{ site.baseurl }}/reference/glossary/#sanitizer
[Bot日志]: {{ site.baseurl }}/getting-started/local-instance/#viewing-logs
