---
layout: post
title: python入门实战融360金融风控大数据竞赛(一)
date: 2016-08-21 15:32:24.000000000 +09:00
categories: python ML 
tags: python 
author: xiaOvan
---

* content
{:toc}

## 引言

>   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;本文适合python初学者阅读，利用python 内置pandas库简易地对融360数据进行处理，并且利用sklearn内置的机器学习算法进行建模。在该文以后此次大赛结束之后再贴上更为合理的模型建立方法及解题思路

## 比赛题目

    融360平台的一端是亿级别有借款需求的小微企业和个人消费者，另一端是有贷款资金的万级别的金融机构（银行、小贷、担保、典当等）和百万级的金融产品，平台通过搜索和推荐服务来撮合借款用户和贷款。在此过程中，用户通过平台的搜索和推荐服务查找适合自己的贷款产品，并填写资料，提交贷款申请，金融机构通过我们的平台收到订单后，对用户资质进行风控审核，决定是否通过用户的订单。用户在平台成功贷款后，有再次贷款需求的用户会通过平台进行二次贷款，对用户是否会二次贷款进行建模预测，可以帮助我们为用户、机构提供更好的服务体验，创造更多的价值。
    我们通过提供平台上的部分数据，希望参赛队伍根据数据，解决问题。
    1、我们将提供的数据包括(出于用户隐私保护的目的，所有用户信息都经过去隐私化处理)：
    a.用户基本信息数据：用户在融360平台上提交的基本资料，如年龄、性别、职业、教育信息等，包含修改记录
    b.用户消费信息数据：用户向融360平台上提交的消费信息数据
    c.用户行为标签数据：用户在融360平台的行为标签。
    d.用户社交关系信息：部分用户社交关系及关系强度。
    2.希望选手解答的问题包括：
    a.根据数据对用户特征进行统计描述，并用可视化的图或表展示。
    b.根据数据分析用户特征与用户是否会二次贷款之间的关系。
    c.结合1)和2)的结论，利用所提供数据建立模型，使用0~1的浮点数给出用户是否会在平台进行二次贷款的可能性。
&nbsp;&nbsp;&nbsp;&nbsp;[比赛链接](http://openresearch.rong360.com/dataanalysis2016/index/)


## A Very Simple Approach

### 数据初览

首先我们利用python的pandas库对数据进行读取:

```python
import pandas as pd

user_info = pd.read_table('/fakepath/user_info.txt',sep=',',header=0)
rong_tag = pd.read_table('/fakepath/rong_tag.txt',sep=',',header=0)
relation1 = pd.read_table('/fakepath/relation1.txt',sep=',',header=0)
relation2 = pd.read_table('/fakepath/relation2.txt',sep=',',header=0)
consum = pd.read_table('/fakepath/consumption_recode.txt',sep=',',header=0)
train =  pd.read_table('/fakepath/train.txt',sep=',',header=0)
test = pd.read_table('/fakepath/test.txt',sep=',',header=0)
```

结合panas.dataFrame的shape及count方法，我们可以先对我们的数据有一个初步的认识,我们拿user_info这个用户基本信息表举例:

```python
user_info.shape
(253006, 22)
user_info.count()
user_id            253006
age                253006
sex                253006
expect_quota       253006
max_month_repay     68648
occupation         253006
education          253006
marital_status     253006
live_info          253006
local_hk           184358
money_function     184358
company_type       253000
salary             253000
school_type        184358
flow               184358
gross_profit       184358
business_type      184358
business_year      184351
personnel_num      184358
pay_type           184318
product_id         253006
tm_encode          253006
dtype: int64
user_info.dtypes
user_id             object
age                 object
sex                  int64
expect_quota         int64
max_month_repay    float64
occupation           int64
education            int64
marital_status       int64
live_info            int64
local_hk           float64
money_function     float64
company_type       float64
salary             float64
school_type        float64
flow               float64
gross_profit       float64
business_type      float64
business_year      float64
personnel_num      float64
pay_type           float64
product_id           int64
tm_encode            int64
dtype: object
user_info.head(5)
user_id|age|sex|expect..
ec76ffd36a9990016ad1e9a4888c3291|41|1|NaN...
...
```

user_info表是一张由253006条纪录，每行纪录包含22列数据构成的纪录，其中由user_info.count()得知哪些列存在空值，这是后续我们数据清洗要处理的对象，另外可以看到user_id及age列均为object,推测user_id为数据库表主键，age列可能存在字符，后续也需要做处理。

```python
user_info['age'].unique()
array(['41', 'NONE', '46', '28', '34', '25', '33', '29', '31', '27', '39',
       '36', '37', '23', '35', '30', '26', '40', '45', '44', '32', '24',
       '22', '21', '42', '48', '20', '38', '19', '53', '47', '49', '43',
       '57', '51', '18', '17', '52', '50', '55', '54', '98', '64', '56',
       '66'], dtype=object)
```

### 我们要预测什么？

    利用所提供数据建立模型，使用0~1的浮点数给出用户是否会在平台进行二次贷款的可能性
    
### 数据处理

题目给了我们几个文件，关于用户的文件主要如下:

1. user_info.txt:包含用户的基本信息
2. rong_tag.txt:包含用户的行为标签数据
3. consumption_recode.txt:包含用户信用卡消费记录
4. relation1.txt:用户关系
5. relation2.txt:带有权重及类别的用户关系

首先我们处理user_info.txt,数据初览后，我们打算先这样简化处理数据

* 对于缺失值，我们暂时用-1填充，这也是一个较好的做法，后续可以改进，对于年龄列的NONE值，我们用年龄列的众数代替

* 对于用户基本信息，我们看到每个用户均有多次的修改记录，通过观察，我们看到tm_encode这个时间列数值越大的数据，用户填写的记录最完整，我们推测tm_encode与更新时间保持一致性，那么我们分组选取时间最大的值作为用户基本信息

```python
#填充NaN值
user_tmp = user_info.fillna(-1)
#替换Age列异常NONE值
user_tmp =recent_user.replace('NONE','-1')
#更新分组内年龄
user_tmp['age'] = user_tmp.groupby(['user_id'])['age'].transform(max)
#分组选出时间最近的用户
grouped_user_info = (user_tmp.assign(temp=user_tmp.groupby(['user_id'])['tm_encode'].rank(method='first', ascending=False)).query('temp<2').sort(['tm_encode','age'],ascending=False))
#删除临时列
del grouped_user_info['temp']
#替换AGE列-1值
age_inx = grouped_user_info[grouped_user_info['age']=='-1']
grouped_user_info[grouped_user_info['age']=='-1']['age'] = grouped_user_info['age'].mode()
```


关于pandas的api,可以参考  [Pandas API Ref](http://pandas.pydata.org/pandas-docs/stable/api.html)

### 特征工程

初步选取除了基本信息以外一下的特征作为

* 用户对于资料的修改次数
* 用户拥有的tag兴趣数量
* 与该用户有联系的用户数量
* 用户的账单数量

为基础数据添加以上的特征

```python
#增加标签计数
rong_df = rong_tag.groupby(['user_id'])['rong_tag'].count().reset_index()
#rong_train = rong_df[rong_df['user_id'].isin(train_x)]
train_input_x.insert(23,'tags',0)
test_input_x.insert(23,'tags',0)
#recent_user.head(5)
train_input_x['tags'] = rong_df['rong_tag'].combine_first(train_input_x['tags'])
#tag_user = train_input_x.reset_index()
test_input_x['tags'] = rong_df['rong_tag'].combine_first(test_input_x['tags'])
#关系计数
rr = relation1.groupby(['user1_id'])['user2_id'].count().reset_index()
train_input_x.insert(24,'rels',0)
test_input_x.insert(24,'rels',0)
train_input_x['rels'] = rr['user2_id'].combine_first(train_input_x['rels'])
#tag_user = train_input_x.reset_index()
test_input_x['rels'] = rr['user2_id'].combine_first(test_input_x['rels'])
#消费账单数
con = consum.groupby(['user_id'])['bill_id'].count().reset_index()
train_input_x.insert(25,'cons',0)
test_input_x.insert(25,'cons',0)
train_input_x['cons'] = con['bill_id'].combine_first(train_input_x['cons'])
#tag_user = train_input_x.reset_index()
test_input_x['cons'] = con['bill_id'].combine_first(test_input_x['cons'])
```

得到最终训练集及测试集

```python
array_feature = ['age','salary','education','marital_status','company_type','money_function','occupation','product_id','tm_encode','tags','rels','cons','modifycount']
#训练集
train_input_x = train_input_x[array_feature]
#测试集
test_input_x = test_input_x[array_feature]
```

为加快模型迭代及收敛，我们使用preproccessing库的Normalizer类对数据进行归一化

```python
X = StandardScaler().fit_transform(train_input_x)
```

### 模型选择

```python
train_user.corr()['lable']
lable             1.000000
sex               0.015212
salary            0.007925
education        -0.007624
marital_status    0.031464
company_type     -0.002416
money_function    0.019309
occupation        0.005891
product_id        0.008261
tags              0.003296
rels             -0.004895
cons             -0.010323
modifycount       0.026394
Name: lable, dtype: float64
```

可以看到并没有较强的线性关系，我们采用另几个机器学习算法试试


```python
from sklearn.linear_model import LogisticRegression
from sklearn.preprocessing import StandardScaler
from sklearn.preprocessing import Normalizer
from sklearn import cross_validation

lr = LogisticRegression()

array_feature = ['age','salary','education','marital_status','company_type','money_function','occupation','product_id','tm_encode','tags','rels','cons','modifycount']
train_input_x_1 = train_input_x[array_feature]
test_input_x_1 = test_input_x[array_feature]
X = StandardScaler().fit_transform(train_input_x_1)
test_X = StandardScaler().fit_transform(test_input_x_1)
lr.fit(X,train_y)
res = lr.predict_proba(test_X)
print(cross_validation.cross_val_score(lr,X,train_y,cv=5))

from sklearn import cross_validation
from sklearn.ensemble import RandomForestClassifier
model = RandomForestClassifier(n_estimators=100,max_features='log2',min_weight_fraction_leaf=0.1)
model.fit(X,train_y)
#scores = cross_validation.cross_val_score(model, X, train_y, cv=3)
expected = train_y
predicted = model.predict(X)

from sklearn.ensemble import AdaBoostClassifier
model = AdaBoostClassifier()
model.fit(X,train_y)

expected = train_y
predicted = model.predict(X)

print(metrics.classification_report(expected, predicted))

from sklearn.neighbors import KNeighborsClassifier
neigh = KNeighborsClassifier(n_neighbors=3)
neigh.fit(X, train_y)

expected = train_y
predicted = model.predict(X)

print(metrics.classification_report(expected, predicted))
print(metrics.confusion_matrix(expected, predicted))


```



### 提交结果

生成预测文件：

```python
import numpy as np
result_p = predict[:,1]
data = np.array(result_p)
df = pd.DataFrame(data)
test_x = pd.DataFrame(test_x)
df = df.rename(columns ={0:'probability'})
#dataframe命名
d = test_x.join(df)
d.to_csv('/fakepath/test_with_random.txt',index=False,header=True)
```

最后，我们用随机森林提交的预测文件能够达到0.5648分，这还算不错，毕竟所有的数据我们都采取的最简化的处理方式，模型也采用最简单的sklearn内置的机器学习算法进行建模，下一步，我们将对数据模型进行更深入的研究。

>&nbsp;&nbsp;&nbsp;&nbsp;您最近一次预测得分为0.5625（2016-08-21），历史最高预测得分为0.5648，目前排名第65位

### 更好的解决办法

题目给出了大量的用户关系的数据，我们可以尝试从社交关系方向对题目进行进一步的学习及处理.另外我们将会对消费数据进行进一步挖掘。


参考资料:

[node2vec](http://snap.stanford.edu/node2vec/)

本文的源代码可从如下GitHub地址获取

[源码](https://github.com/xiaOvan/r360)

如转载，请注明[本文](http://xiaovan.github.io/2016/08/21/a-simple-approach-r360-competition-python-sklearn/)出处
