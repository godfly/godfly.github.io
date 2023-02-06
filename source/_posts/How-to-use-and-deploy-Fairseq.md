---
title: How to use and deploy Fairseq
date: 2023-01-31 20:58:20
toc: true
categories: 
- Tech
- AI
tags: 
- NLP
- Translation
---

本文记录了我使用[FairSeq](https://github.com/facebookresearch/fairseq)训练、测试和部署**中英翻译**demo的整个流程和遇到的坑，同时文章中分享了一部分中英数据集。

<!-- more -->

## What's FairSeq

相信通过搜索引擎来到这的朋友不必多言，但是我还是要简单介绍一下FairSeq。

Fairseq在文本生成/机器翻译开源框架中是佼佼者（其他还有`tensor2tensor`，`OpenNMT`等），其作为开源的NLP工具库是对pytorch的上层封装，其基础代码也是通过pytorch编写，有非常多的特性以及性能优化，并且它的代码规范和设计模式也非常值得一读。

## 1. 环境安装

本文的基本环境为：
- python=3.8
- CUDA=11.0
- cudnn=8.0
- Fairseq=1.11.0
- pytorch=
- tensorflow=

requirment.txt
```
tensorflow=
```

安装Fairseq
```
git clone https://github.com/pytorch/fairseq
cd fairseq
pip install --editable ./
```

## 2. 整体流程图

## 3. 数据部分

### 1. 数据获取
### 2. 数据预处理

## 4.模型部分
### 1.训练 
### 2.预测

## 5.测试部分

## 6.部署部分

## 7.FAQ

## Reference