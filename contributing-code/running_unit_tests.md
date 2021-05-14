---
layout: default
title: Running unit tests
parent: Contributing code
nav_order: 2
permalink: /contributing-code/running-unit-tests/
---

# Running unit tests

- TOC
{:toc}
---

## Test core changes

您可以使用以下命令运行针对核心功能的单元测试: 

### Python code

```bash
python butler.py py_unittest -t core
```

可以使用的可选开关: 
* `-m`: 并行地执行测试(推荐). 
* `-v`: 以详细模式(INFO日志级别)运行测试.
* `-u`: 显示`print`的输出(利于调试). 
* `-p <test_name/test_prefix_with_wildcards>`: 执行一个特定的测试或一组与特定前缀匹配的测试. 例如:`-p libfuzzer_ *`将执行所有以libFuzzer为前缀命名的单元测试. 

## Test App Engine changes

大多数App Engine代码都是用Python编写的. 您可以使用以下命令对App Engine的改动(例如UI,cron)运行单元测试. 

```bash
python butler.py py_unittest -t appengine
```

您同样可以使用上述在[Python code]段中定义的任何开关. 

[Python code]:  {{ site.baseurl }}/contributing-code/running-unit-tests/#python-code