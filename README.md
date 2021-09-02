# Clusterfuzz官方文档中文翻译

Clusterfuzz是Google [OSS-Fuzz](https://github.com/google/oss-fuzz)项目里的模糊测试后端, 也是目前开源的少数模糊测试的基础设施. 

目前关于Clusterfuzz的资料还相对较少, 也为了自己方便查阅, 故而对文档(v2.4.0)进行了翻译和部署, 欢迎感兴趣的伙伴提交PR. 

在线阅读地址: [ClusterFuzz中文文档](https://vancir.github.io/clusterfuzz-document-cn/)

## 本地部署

```bash
sudo apt install ruby bundler
bundle install --path vendor/bundle
bundle exec jekyll serve
```

## 翻译进度

| 状态 | 篇章名                                                       | 进度     |
| ---- | ------------------------------------------------------------ | -------- |
| ✅    | ClusterFuzz                                                  | 已完成   |
| ✅    | ClusterFuzz - Architecture                                   | 已完成   |
| ✅    | Getting started                                              | 已完成   |
| ✅    | Getting started - Prerequisites                              | 已完成   |
| ✅    | Getting started - Running a local instance                   | 已完成   |
| ✅    | Setting up fuzzing                                           | 已完成   |
| ✅    | Setting up fuzzing - libFuzzer and AFL                       | 已完成 |
| ✅    | Setting up fuzzing - Blackbox fuzzing                        | 已完成   |
| ✅    | Setting up fuzzing - Heartbleed example                      | 已完成   |
| ✅    | Production setup                                             | 已完成   |
|      | Production setup - ClusterFuzz                               | 未开始   |
|      | Production setup - Build pipeline                            | 未开始   |
|      | Production setup - Setting up a fuzzing job                  | 未开始   |
|      | Production setup - Setting up bots                           | 未开始   |
| ✅    | Using ClusterFuzz                                            | 未开始   |
| ⏸️    | Using ClusterFuzz - UI overview                              | 正在进行 |
| ✅    | Using ClusterFuzz - Workflows                                | 已完成   |
| ✅    | Using ClusterFuzz - Workflows - Triaging new crashes         | 已完成   |
| ✅     | Using ClusterFuzz - Workflows - Fixing a bug                 | 已完成   |
| ✅     | Using ClusterFuzz - Workflows - Analyzing fuzzer performance | 已完成   |
| ✅     | Using ClusterFuzz - Workflows - Uploading a testcase         | 已完成   |
| ✅    | Using ClusterFuzz -  Advanced features                       | 已完成   |
| ✅     | Using ClusterFuzz -  Advanced features - Access control      | 已完成   |
|      | Using ClusterFuzz -  Advanced features - Code coverage       | 未开始   |
| ✅    | Contributing code                                            | 已完成   |
| ✅    | Contributing code - Source code                              | 已完成   |
| ✅    | Contributing code - Running unit tests                       | 已完成   |
| ✅    | Contributing code - Staging changes                          | 已完成   |
| ✅    | Reference                                                    | 已完成   |
| ✅    | Reference - Glossary                                         | 已完成   |
| ✅    | Reference - Coverage guided vs blackbox fuzzing              | 已完成   |
| ⏸️    | Reference - Job definition                                   | 正在进行 |
| ⏸️    | Reference - FAQ                                              | 正在进行 |

## 文档主题

Clusterfuzz文档采用[just the docs](https://pmarsceill.github.io/just-the-docs/)主题进行构建.