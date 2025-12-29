# TQE: Translation-based Query Expansion

TQE（Translation-based Query Expansion）是一个 **基于等价翻译的查询扩展模块**，用于多语言检索系统。  
该项目仅通过 **忠实翻译（faithful translation）** 扩展用户查询，不进行语义改写或意图推断，避免引入语义漂移，适用于 RAG 与多级检索系统。

---

## Motivation

在许多检索系统中，Query Rewrite 往往依赖大模型进行语义重写或扩写，这在工程实践中容易带来以下问题：

- 查询语义发生漂移（intent drift）
- 与 label / index 的原生表达不一致
- 系统行为不可控，debug 成本高
- 额外引入大模型调用，增加延迟与成本

另一方面，当前主流的 embedding 模型与大语言模型本身已基于 **多语言语料训练**，不同语言的等价表达在语义空间中天然对齐。

基于这一事实，TQE 采用一种更克制、更工程友好的策略：

> **只做等价翻译，不做语义改写。**

---

## Core Idea

给定一个用户查询 `q`，TQE 生成一组 **语义等价、语言不同** 的查询集合：

- 原始查询（如中文）
- 翻译后的等价查询（如英文）

所有查询在语义上等价，仅用于从不同语言视角命中同一语义空间中的相关文档。

该策略特别适用于：
- 中文用户检索英文语料
- label / 索引语言为英文的系统
- 多级检索（label → doc → chunk）
- 向量检索 + rerank 场景

---

## Features

- ✅ 仅做忠实翻译，不进行语义扩写
- ✅ 支持不可翻译项保护（label、代码、error message、版本号等）
- ✅ 轻量、可插拔的翻译接口设计
- ✅ 不依赖特定模型或框架
- ✅ 工程友好，行为可预测

---

## Project Structure

```text
tqe/
├── README.md
├── pyproject.toml / requirements.txt
├── tqe/
│   ├── __init__.py
│   ├── translator.py    # 翻译接口（支持不可翻译项保护）
│   ├── rewrite.py       # Translation-based Query Expansion 实现
│   ├── utils.py
│   └── demo.py          # 本地可运行示例
└── .gitignore
