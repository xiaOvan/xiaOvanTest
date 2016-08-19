---
layout: post
title: feature engineering
date: 2016-05-16 15:32:24.000000000 +09:00
categories: ML sklearn
tags: 特征工程 sklearn
author: xiaOvan
---

* content
{:toc}

## 1.特征工程是什么

  引用quora上的回答:
  
>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;When working on a machine learning problem, feature engineering is manually designing what the input x's should be.  In the case of visual object recognition problems, when you're doing something other than feeding raw pixels into the classifier, you're engineering features.  You might expect that edge orientations and color histograms are going to help discriminate "orange balls" vs. "background patterns" so you might manually compute these quantities and then feed them into the machine learning black-box.  And quite often you will actually get much much better performance when you reason about your vision problem and use the features which you think might help.  This is called feature engineering.  HOG, SIFT, GIST and all those popular computer vision features are the result of feature engineering.

>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;"Feature engineering" these days gets a bad reputation in vision because computer vision folks have been known to spend thousands of hours doing this.  It doesn't feel like the glory of Machine Learning or Artificial Intelligence, but when you just need your robot to pick up that orange ball, a bit of feature engineering can go a long way.

>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;A lot of what was typically done by a PhD student can now be done by machine.  Look into Deep Learning if you're interested in learning more.  However, the deep architectures and the learning algorithm parameters must still be tinkered with when applying such models to new domains, so I guess it's fair to call that architecture engineering.

>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;So if you're playing with pixels and creating new x's, you're doing feature engineering.  If you're one of those that's proud to be using a system which learns features automatically, but you're playing with activation functions, layer topologies, and learning rates, then you're architecture engineering.  At the end of the day, there still needs to be a smart person (usually a PhD) that is twiddling some knobs (anywhere between 1 day and 10 years) and the resulting awesome-performance you'll obtain will be both a triumph for man and machine.

![image](http://o7q84v6xt.bkt.clouddn.com/feature-engineering.jpg)

## 2.数据预处理

## 3.特征选择

## 4.降维

## 5.总结

## 6.本文参考

  [本文参考](http://www.cnblogs.com/jasonfreak/p/5448385.html)


