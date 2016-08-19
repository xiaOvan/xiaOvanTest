---
layout: post
title: xgboost paper 发布
date: 2016-08-16 15:32:24.000000000 +09:00
categories: ML
tags: KDD XGBOOST
author: xiaOvan
---

* content
{:toc}

## xgboost 介绍
    目前Kaggle上最火的算法。
    xgboost的全称是eXtreme Gradient Boosting。它是Gradient Boosting Machine的一个c++实现。xgboost最大的特点在于，它能够自动利用CPU的多线程进行并行，同时在算法上加以改进提高了精度。在数据建模中，当我们有数个连续值特征时，Boosting分类器是最常用的非线性分类器。它将成百上千个分类准确率较低的树模型组合起来，成为一个准确率很高的模型。这个模型会不断地迭代，每次迭代就生成一颗新的树。然而，在数据集较大较复杂的时候，我们可能需要几千次迭代运算，这将造成巨大的计算瓶颈。
    xgboost正是为了解决这个瓶颈而提出。单机它采用多线程来加速树的构建，并依赖深盟的另一个部件rabbit来进行分布式计算。
    由于其高效的C++实现，xgboost在性能上超过了最常用使用的R包gbm和Python包sklearn。例如在Kaggle的希格斯子竞赛数据上，单线程xgboost比其他两个包均要快出50%，在多线程上xgboost更是有接近线性的性能提升。由于其性能和使用便利性，xgboost已经在Kaggle竞赛中被广泛使用。
    
## xgboost Paper

Paper 下载地址：

[A scalable tree boosting system](http://www.kdd.org/kdd2016/papers/files/rfp0697-chenAemb.pdf)

## python 安装步骤

### 使用pip安装

终端输入:

```shell
pip install xgboost

```

因为安装包含有C++源码，所以在平台上安装之前需要C++编译器

* MAC:  install gcc from brew by brew tap homebrew/versions; brew install gcc --without-multilib
* Linux: install gcc by sudo apt-get install build-essential
* Windows:从源码包安装吧

### 使用源码安装

```shell
git clone --recursive https://github.com/dmlc/xgboost
cd xgboost
make -j4
```

[installation guide](https://xgboost.readthedocs.io/en/latest/build.html)

## xgboost 使用(Python,R)

### python

```python
import xgboost as xgb
# read in data
dtrain = xgb.DMatrix('demo/data/agaricus.txt.train')
dtest = xgb.DMatrix('demo/data/agaricus.txt.test')
# specify parameters via map
param = {'max_depth':2, 'eta':1, 'silent':1, 'objective':'binary:logistic' }
num_round = 2
bst = xgb.train(param, dtrain, num_round)
# make prediction
preds = bst.predict(dtest)
```

### R

```R
# load data
data(agaricus.train, package='xgboost')
data(agaricus.test, package='xgboost')
train <- agaricus.train
test <- agaricus.test
# fit model
bst <- xgboost(data = train$data, label = train$label, max.depth = 2, eta = 1, nround = 2,
               nthread = 2, objective = "binary:logistic")
# predict
pred <- predict(bst, test$data)
```

##其他资料

* [how to develop ur first xgboost model in python](http://machinelearningmastery.com/develop-first-xgboost-model-python-scikit-learn/)
* [快速上手：在R中使用XGBoost算法](http://www.tuicool.com/articles/MFn6by#0-tsina-1-21840-397232819ff9a47a7b7e80a40613cfe1)
* [核心风控模型技术解析：XGBoost模型简介](http://weibo.com/ttarticle/p/show?id=2309403996478942140088)

