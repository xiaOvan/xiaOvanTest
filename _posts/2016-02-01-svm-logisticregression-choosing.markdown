---
layout: post
title: SVM及逻辑回归在建模时候的选择
date: 2016-02-01 15:32:24.000000000 +09:00
categories: ML
tags: ML
author: xiaOvan
---

According to Andrew Ng
---



* 如果Feature的数量很大，跟样本数量差不多，这时候选用LR或者是Linear Kernel的SVM
* 如果Feature的数量比较小，样本数量一般，不算大也不算小，选用SVM+Gaussian Kernel
* 如果Feature的数量比较小，而样本数量很多，需要手工添加一些feature变成第一种情况

另外迭代求loss function 最大(小)值是采用牛顿迭代法还是梯度下降（批量，随机）:

* 如果feature n小与1000，适宜选择牛顿迭代
* 如果feature n大于1000，适宜选择梯度下降

因为牛顿迭代的空间复杂度为$O(n^3)$,时间复杂度较小，梯度下降需要较多次迭代出极值，而空间复杂度为$O(n)$.


另参考资料

[国立台湾大学PDF][ref]

[SVM and logistic regression][ref1]

[ref]: http://www.csie.ntu.edu.tw/~cjlin/talks/msri.pdf

[ref1]: http://charlesx.top/2016/03/LR-SVM/
