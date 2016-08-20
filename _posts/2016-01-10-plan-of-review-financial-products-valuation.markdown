---
layout: post
title: 金融市场产品估值方法回顾
date: 2016-01-10 15:32:24.000000000 +09:00
categories: financial_products model_valuation
tags: model algorithm financial products derivatives foreignexchange
author: xiaOvan
---

* content
{:toc}

## 回顾计划

        因准备FRM考试，在这里回顾一下金融市场产品估值模型及背后业务含义，并且包括实现过程。


## 估值模型准备工作

### 术语(Terms):

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

  具体术语到产品估值模型介绍时再详细介绍,本文将介绍的金融市场产品如下:

* Spot
* Forward
* Swap
* Fixed Rate Bond
* Float Rate Bond
* Interest Rate Swap
* Options

### 常见估值模型

#### Discounted Cash Flow(DCF)
    
    金融市场产品最常见及最常用的模型，没有之一，现金流折现是将按照标的产品未来的价值按照折线利率进行着先到估值日期，如果折现到估值日期后的价格大于目前标的产品的市场价格，那该产品为一个优秀的标的产品
    计算方法:
    DCF = 
    
### Two or Three Factor Model

### B/S Model

### 0.通用参数及惯例

#### 0.1 日期惯例

#### 0.2 利率曲线

#### 0.3 计息惯例
 
### 1.外汇即期

#### 1.1 产品定义

    外汇即期(Spot Currency Swap:指外汇买卖（以约定的汇率进行货币交换）成交后，交易双方与当天或者两个交易日内完成交割(deliver)的交易行为,通常属于场内交易，有实际的货币或商品互换行为。

#### 1.2 估值方法

    场景：我有1000美元的名义本金，我与我的交易对手(counterparty)以美元对人民币1:6.8（USD/CNY=6.8)的价格进行货币互换，那么我能买入到6800的人民币，而卖出1000美金，所以我的即期合约估值价格为：
    ValuationOfSpot = Notional * spotRate
    因即期一般交割日为当日或者两个工作日内，所以一般把overnight和spotnight的汇率报价认为是该货币互换的估值价格。
    假设实际市场汇率为 MarketRate，那么我在此次交易中我的损益(Profit & Loss)即为
    P&L = Notional * (spotRate - MarketRate)

### 2.外汇远期

#### 2.1 产品定义

    外汇远期(Forward Currency Swap):交易双方在未来的某一个时间点以商定好的汇率及头寸进行货币互换，远期往往用来对冲在到期日来临之前因汇率变动过大造成的风险，常见的是在即期货币互换完成后，签订一笔远期货币互换协议来对冲汇率风险。
    
#### 2.2 估值方法
    
    采用Cashflow Discount方法

```math
E = MC^2
```

### 3.外汇掉期

#### 3.1 产品定义

#### 3.2 估值方法
        
### 4.固定利率债券

#### 4.1 产品定义

#### 4.2 估值方法

        fixed rate bond

### 5.浮动利率债券

#### 5.1 产品定义

#### 5.2 估值方法

        float rate bond

### 6.利率互换协议

#### 6.1 产品定义

#### 6.2 估值方法       

        irs
        
### 7.欧式(美式)期权

#### 7.1 产品定义

#### 7.2 估值方法

        
### 8.总结

### 9.参考资料
