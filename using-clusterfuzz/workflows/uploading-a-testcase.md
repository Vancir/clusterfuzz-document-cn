---
layout: default
title: Uploading a testcase
permalink: /using-clusterfuzz/workflows/uploading-a-testcase/
nav_order: 4
parent: Workflows
grand_parent: Using ClusterFuzz
---

# Uploading a testcase

ClusterFuzz提供了*Upload Testcase*页面，用来在最新的生产用构建版本中运行一个测试用例，并检查是否生成crashes。上传测试用例*页面可以运行测试用例的二进制文件，并提供有关crashes的细节。如crashes的栈回溯，crashes被引入生成的时间，等等。

- TOC
{:toc}

---

## Upload new testcase

为了上传一个新的测试用例：

1. 点击“上传”按钮

2. 获取本地测试用例
   1. 如果测试用例是一个文件，直接上传即可。
   2. 如果测试用例包含多个文件：
     - 传递给应用程序的*main文件*必须在其名称中包含**run**（例如run.html）。
     - 将所有的文件打包在一个压缩包里。支持的压缩包格式包括*zip*和*tar*。
     - **异常**。如果你想同时测试多个测试用例，你不需要重命名它们。只要把它们添加在一个压缩包文件中，并在表格中选择 **独立测试压缩包中的每个文件**的复选框。
   
3. 点击 "选择文件 "按钮，在文件选择器对话框中提供测试用例的压缩包文件。

4. 选择一个**Job**。可以提供哪一个版本构建或应用程序的信息
   运行这个测试用例。
   
5. 如果你在上一步选择了一个fuzzing引擎工作（例如*libFuzzer*，*AFL*）。你需要提供要使用的*fuzz target*的名称。因为一个应用程序的构建版本可能包含多个fuzz target二进制文件。

6. 为表格中的其他可选字段提供数值。比如：
     1. 可以提供一个*Commit Position/Revision*来运行
        特定的[revision]。这通常用于检查应用程序的旧版本的crash。
     2. 如果需要http服务器运行测试用例，可以勾选**通过HTTP服务器加载测试用例**。

7. 点击“创建”按钮。

## Check status

一旦上传了一个新的测试用例，就会跳转到*测试用例细节*页面。这个页面每隔*5分钟*自动刷新一次，以提供最新的结果。起初，ClusterFuzz会尝试查找该测试用例是否生成crashes。

如果测试用例没有生成crashes，ClusterFuzz会将测试用例的状态设置为***不可复现**。如果测试用例生成了crashes，那么ClusterFuzz会首先更新在*Overview*部分的crash参数以及**崩溃栈回溯**。然后，ClusterFuzz会尝试其他任务，如测试用例的[最小化]、查找[回归范围]等。

请耐心等待结果。结果的速度将取决于决于bots的效率。

[revision]: {{ site.baseurl }}/reference/glossary/#revision
[最小化]: {{ site.baseurl }}/reference/glossary/#minimization
[回归范围]: {{ site.baseurl }}/reference/glossary/#regression-range
