---
layout: post
title: 利用python实现几个排序算法
date: 2015-12-22
categories: algorithm
tags: python sorting algorithm
author: xiaOvan
---

* content 
{:toc}

## 1.选择排序

代码:

```python
def selectionsort(lst):
        if len(lst)<=2:
                return []
        for i in range(0,len(lst)):
                smallest=lst[i]
                pos=i
                for j in range(i,len(lst)):
                        if lst[j]<smallest:
                                smallest=lst[j]
                                pos=j
                if i!=pos:
                        lst[i],lst[pos] = lst[pos],lst[i]
        return lst
```

## 2.冒泡排序

代码:

```python
def bubblesort(lst):
        for i in range(0,len(lst)):
                for t in range(i,len(lst)):
                        if(lst[i]<=lst[t]):
                                lst[i],lst[t]=lst[t],lst[i]
        return lst
```

## 3.插入排序

代码:

```python
def insertsort(lst):
        if len(lst)<=2:
                return []
        for i in range(0,len(lst)):
                key = lst[i]
                j = i - 1
                while j>=0 and lst[j]>key:
                        lst[j+1] = lst[j]
                        j -= 1
                lst[j+1] = key
        return lst
```

To be Continued....

