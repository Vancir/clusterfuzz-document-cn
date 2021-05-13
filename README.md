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
| ⏸️    | Setting up fuzzing - libFuzzer and AFL                       | 正在进行 |
| ❎    | Setting up fuzzing - Blackbox fuzzing                        | 未开始   |
| ✅    | Setting up fuzzing - Heartbleed example                      | 已完成   |
| ❎    | Production setup                                             | 未开始   |
| ❎    | Production setup - ClusterFuzz                               | 未开始   |
| ❎    | Production setup - Build pipeline                            | 未开始   |
| ❎    | Production setup - Setting up a fuzzing job                  | 未开始   |
| ❎    | Production setup - Setting up bots                           | 未开始   |
| ❎    | Using ClusterFuzz                                            | 未开始   |
| ❎    | Using ClusterFuzz - UI overview                              | 未开始   |
| ❎    | Using ClusterFuzz - Workflows                                | 未开始   |
| ❎    | Using ClusterFuzz - Workflows - Triaging new crashes         | 未开始   |
| ❎    | Using ClusterFuzz - Workflows - Fixing a bug                 | 未开始   |
| ❎    | Using ClusterFuzz - Workflows - Analyzing fuzzer performance | 未开始   |
| ❎    | Using ClusterFuzz - Workflows - Uploading a testcase         | 未开始   |
| ❎    | Using ClusterFuzz -  Advanced features                       | 未开始   |
| ❎    | Using ClusterFuzz -  Advanced features - Access control      | 未开始   |
| ❎    | Using ClusterFuzz -  Advanced features - Code coverage       | 未开始   |
| ❎    | Contributing code                                            | 未开始   |
| ❎    | Contributing code - Source code                              | 未开始   |
| ❎    | Contributing code - Running unit tests                       | 未开始   |
| ❎    | Contributing code - Staging changes                          | 未开始   |
| ❎    | Reference                                                    | 未开始   |
| ❎    | Reference - Glossary                                         | 未开始   |
| ❎    | Reference - Coverage guided vs blackbox fuzzing              | 未开始   |
| ❎    | Reference - Job definition                                   | 未开始   |
| ❎    | Reference - FAQ                                              | 未开始   |

## 文档主题

Clusterfuzz文档采用[just the docs](https://pmarsceill.github.io/just-the-docs/)主题进行构建.