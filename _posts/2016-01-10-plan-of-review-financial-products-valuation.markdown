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
    
      这里介绍估值模型中重要的日期，其他例如产品特有的issue_date等日期暂不列出
* 汇率(Currency Rate)
    + spot rate:即期汇率(currency exchange rate) 例如 CNY/USD = 6.8 
    + forward rate:远期汇率(currency exchage rate in the future)
* 利率(Interest Rate)
    + par rate:市场利率
    + sport/forward rate:即期/远期利率
    + coupon rate:票面利率
    + strike rate:行权利率
* 价格(Price)
    + market price:市场价格，含利息
    + clean price:净价，不含利息
    + strike price:行权价格
    + face value:（债券）票面价格
    + bid/offer/mid: 

  具体术语到产品估值模型介绍时再详细介绍
 
### 0.通用参数及惯例

#### 0.1 日期惯例

#### 0.2 利率曲线

#### 0.3 计息惯例
 
### 1.外汇即期

#### 1.1 产品定义

    外汇即期(Spot Exchange Transactions):指外汇买卖（以约定的汇率进行货币交换）成交后，交易双方与当天或者两个交易日内完成交割(deliver)的交易行为,通常属于场内交易，有实际的货币或商品互换行为。

#### 1.2 估值方法

    场景：我有1000美元的名义本金，我与我的交易对手以美元对人民币1:6.8（USD/CNY=6.8)的价格进行货币互换，那么我能买入到6800的人民币，而卖出1000美金，所以我的即期合约估值价格为：
    
    ValuationOfSpot = Notional * spotRate

    假设实际市场汇率为 MarketRate，那么我在此次交易中我的损益即为
    
    P&L = Notional * (spotRate - MarketRate)

### 2.外汇远期

#### 2.1 产品定义
    外汇远期
#### 2.2 估值方法

### 外汇掉期

        swap
        
### 固定利率债券

        fixed rate bond

### 浮动利率债券

        float rate bond

### 利率互换协议
        
        irs
        
### 欧式(美式)期权

        定义
