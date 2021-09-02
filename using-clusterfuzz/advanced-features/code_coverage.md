---
layout: default
title: Code coverage
parent: Advanced features
grand_parent: Using ClusterFuzz
permalink: /using-clusterfuzz/advanced/code-coverage/
nav_order: 2
---

# Code coverage
本文档描述了使用ClusterFuzz的项目启用代码覆盖率报告的要求和建议。

- TOC
{:toc}

---

## ClusterFuzz and code coverage
ClusterFuzz能够存储、展示和利用代码覆盖率信息。然而，ClusterFuzz并不能生成代码覆盖率报告，因为该过程取决于项目所使用的构建系统版本，而不同项目的构建系统版本可能有很大不同。

## Setting up a code coverage builder
可以设置一个外部构建工具或持续集成job，为ClusterFuzz生成代码覆盖数据。对于C和C++项目，建议使用 [Clang Source-based Code Coverage](https://clang.llvm.org/docs/SourceBasedCodeCoverage.html).

## Code coverage report and stats
构建工具典型的工作流程如下:
1. 检查项目源码最新版本
2. 构建项目中所有支持代码覆盖率插桩的Fuzzer
3. 从ClusterFuzz下载最新的语料库备份
4. 为项目中的每个fuzzer：
  - 解压缩语料库备份
  - fuzzer运行解压缩后的语料库
  - 使用`llvm-profdata merge`处理覆盖率转储（`.profraw`文件）
  - 使用生成的`.profdata`文件，通过`llvm-cov export -summary-only`方式生成 `$fuzzer_name.json`文件 。

5. 使用`llvm-profdata merge`将各个fuzzers产生的所有`.profdata` 文件合并成一个`.profdata` 文件。
6. 使用`.profdata`文件为整个项目生成`summary.json`文件。生成的文件将包括所有fuzzers的汇总数据。
7. 使用`.profdata`文件，用`llvm-cov show -format=html`生成HTML报告。该报告将包括所有fuzzers的汇总数据。

因此，构建工具应该生成以下内容：
1. 项目中每个fuzzer的统计覆盖率的JSON文件。
2. 整个项目统计覆盖率的JSON文件。
3. 整个项目的HTML报告。

[这](https://github.com/google/oss-fuzz/blob/master/infra/gcb/build_and_run_coverage.py)是[Google Cloud Build](https://cloud.google.com/cloud-build/)用于定义OSS-Fuzz代码覆盖率的一个例子，利用Chromium中的[coverage_utils.py](https://cs.chromium.org/chromium/src/tools/code_coverage/coverage_utils.py)脚本和[this bash script](https://github.com/google/oss-fuzz/blob/master/infra/base-images/base-runner/coverage)脚本。

## Coverage information file
构建工具需要将Code coverage report and stats中提到的三个文件，以及包含覆盖率信息的JSON文件上传至项目配置中指定的[GCS bucket](https://cloud.google.com/storage/docs/creating-buckets)。文件名应等于项目名称，例如：`zlib.json`。该JSON文件必须上传至以下位置：

```bash
gs://<bucket name>/latest_report_info/<project name>.json
# Example from OSS-Fuzz:
gs://oss-fuzz-coverage/latest_report_info/zlib.json
```
以下为文件格式：

```json
{
    "report_date": "YYYYMMDD",
    "fuzzer_stats_dir": "gs://path_to_directory_with_per_fuzzer_summary.json_files",
    "report_summary_path": "gs://path_to_the_project_summary.json_file",
    "html_report_url": "https://link_to_the_main_page_of_the_report",
}
```

* `report_date` 为报告生成的数据
* `fuzzer_stats_dir`是一个GCS目录，包含每个fuzzer的JSON文件 (`$fuzzer_name.json`)。
* `report_summary_path`应该指向`summary.json`文件，包括项目中所有fuzzers的综合数据。
* `html_report_url`应该指向HTML报告的`index.html`。

 `zlib.json` 文件的例子，由代码覆盖job上传至 OSS-Fuzz。

```json
{
    "report_date": "20190112",
    "fuzzer_stats_dir": "gs://oss-fuzz-coverage/zlib/fuzzer_stats/20190112",
    "report_summary_path": "gs://oss-fuzz-coverage/zlib/reports/20190112/linux/summary.json",
    "html_report_url": "https://storage.googleapis.com/oss-fuzz-coverage/zlib/reports/20190112/linux/index.html",
}
```
