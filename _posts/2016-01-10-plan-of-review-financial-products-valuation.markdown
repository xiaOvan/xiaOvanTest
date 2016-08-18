---
layout: post
title: 金融市场产品估值及方法回顾
date: 2016-01-10 15:32:24.000000000 +09:00
categories: financial_products
tags: model algorithm financial products derivatives foreignexchange
author: xiaOvan
---

* content
{:toc}

## 回顾计划

        因准备FRM考试，在这里回顾一下金融市场产品估值模型及背后业务含义，并且包括实现过程。


## 金融产品介绍

  一些术语(Terms):

* 日期(Dates)
    + maturity_date:产品到期日期
    + valuation_date:估值日期，即估算标的产品价格的日期
    + settlement_date:交割日
    + effective_date：
    + strike_date：期权或者含权产品的行权日期
* 汇率(Currency Rate)
    + spot rate:即期汇率 例如 CNY/USD = 6.8 
    + forward rate:远期汇率 
* 利率(Interest Rate)
    + par rate:市场利率
    + sport/forward rate:即期/远期利率
    + coupon rate:票面利率
    + strike rate:行权利率
* 价格(Price)
    + market price:市场价格，含利息
    + clean price:净价，不含利息
    + strike price:行权价格
    + par value:（债券）票面价格
 
### 1.外汇即期

#### 1.1 产品定义

    外汇即期(Spot):

#### 1.2 估值方法

    ValuationOfSpot = Notional * spotRate

### 2.外汇远期

        forward

### 外汇掉期

        swap
        
### 固定利率债券

        fixed rate bond

### 浮动利率债券

        float rate bond

### 利率互换协议
        
        irs
        
### 欧式(美式)期权

