---
weight: 3
title: "EHS_Scrutinizer开发者文档"
date: 2024-02-15T07:11:40.000Z
lastmod: 2024-02-15T12:45:58.000Z
draft: false
author: "玉汝尘"
authorLink: "https://administroot-blog.netlify.app/zh-cn"
description: "EHS_Scrutinizer开发者文档" 
resources:
- name: featured-image
  src: profile.jpg
- name: featured-image-preview
  src: profile.jpg

tags: ["EHS", "EHS_Scrutinizer"]
categories: ["EHS_Scrutinizer"]

toc:
  auto: true

twemoji: false
lightgallery: true
---

EHS_Scrutinizer开发者文档

<!--more-->

## 招贤纳士

目前，项目处于起步阶段，需要各方面的能人巧匠，你可以通过[主页的联系方式](https://administroot-blog.netlify.app/zh-cn/)或线下咨询详情。

前提条件：有相关的EHS工作经验。

| 职位 | 技术栈 |
| :----: | :----: |
| Python工程师 | Peewee, Sqlite, Pytest |
| C/C++工程师 | Qt, CMake, GUI设计 |
| 核对校审 | 录入、核对、同步GBZ相关信息，表格设计 |

## 原理

流程图如下：

{{< mermaid >}}
graph LR;
    A[填写模板表格] -->|清洗| B(预处理过滤器)
    B --> |清洗| C(通用预处理过滤器)
    C --> |入库| D(Sqlite)
    D --> |查询| E(后处理过滤器)
    E --> |清洗| F(通用后处理过滤器)
    F --> |写入| G(EXCEL表格)
{{< /mermaid >}}

1. 从用户填写的EXCEL表格（模板.xlsx）中获取信息。
2. 通过预处理过滤器清洗数据，出现概念问题（如同一操作位接害时间不同）提醒用户，并决定是否继续流程。并进行专门化的数据处理。
3. 通过通用预处理过滤器清洗数据，确保数据原子性。
4. 数据入Sqlite数据库。
5. Sql查询，通过后处理过滤器清洗数据，并进行专门化的数据处理。
6. 通过通用后处理过滤器清洗数据。
7. 数据写入EXCEL表格。

## 测试

### 命令

```shell
pytest test.py
```

### 用例

- 测试用例路径：/test/test-NUM.xlsx  /test/res-NUM.xlsx

## 注意事项

- 所有*PR*在合并主线前先Approve，后过**CI**，如果**CI**未过，请自行check。
- 涉及到**新表格（Sqlite和EXCEL）**的开发，请在**测试目录**下的测试EXCEL中新增相应的测试sheet；其他开发能过**CI**即可。
- 确保**EXCEL Sheet名称**与**Sqlite table名称**一致，便于分辨和开发。
- :warning:**禁止**EXCEL Sheet名称、Sqlite table名称、字段名有任何的**中文**字符，请翻译为**英文**；输出结果的pd.Dataframe{}和EXCEL除外。

## 常见问题

**Q**：为什么要用Sqlite，不能直接内存中读写吗？

**A**：SQL使得许多的逻辑无须自己实现，在代码结构上更加易读、便于分层设计。在多表联查等复杂环境中，SQL的优势更加明显。

**Q**：我不懂技术，想参与项目，该做些什么准备？

**A**：你可以参与核对校审工作，对GBZ相关信息进行录入、核对、同步（比如GBZ 2.1 / GBZ 2.2），设计程序输出表格。在开展工作前，你需要了解一些**Git**的概念，掌握**Git**的基本使用方法（比如上文提到的PR、CI，以及Merge、Commit、Push等）。当然如果你想参与技术开发，需要自学一下相关的*技术栈*。
